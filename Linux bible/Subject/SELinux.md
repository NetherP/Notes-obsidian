Role Based Access Control. It means each subject(user/process) have a role. The resource(file) have types. A role have rules that allow or deny them access to certain file type.

To see SELinux permission on file  add `-Z` to `-ls -l`:
```bash
ls -lZ my_stuff
-rw-rw-r--. johndoe johndoe unconfined_u:object_r:user_home_t:s0 ... my_stuff

#user security context
id
uid=1000(johndoe) gid=1000(johndoe) groups=1000(johndoe) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

#process security context
ps -eZ
unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 1589 pts/0 00:00:00 bash
```
It display four item, 
user(`unconfined_u`)
role(`object_r`)
type(`user_home_t`)
level(`s0`)
They're together called security context.


### Operational Modes
Three operational Mode:
- disabled: off
- permissive: only logs the breach, not enforced `audit2allow` to read logs and change the policy
- enforcing: on
edit it in `/etc/selinux/config` file


### Policy rules
the doc package:
```bash
yum install selinux-policy-doc
apt install selinux-policy-doc
```
Access the documentation in the form of HTML files in: 
`file:///usr/share/doc/selinux-policy/html/index.html` for fedora RHEL
`file:///usr/share/doc/selinux-policy-doc/html/index.html` for ubuntu.

### check current mode
```bash
getenforce #just operation modes
sestatus #various status
```

### set mode
```bash
setenforce enforcing #or 1
setenforce permissive #or 0
```
Set temporarily. It read from the config file upon reboot. 

### Check security context
```bash
secon
secon -urt
secon -urt -p 1
secon -urt -f /etc/passwd
```
secon by itself see all security context for the current process.
#### options
`-u`: user
`-r`: role
`-t`: type
`-s`: sensitivity level
`-c`: clearance level
`-m`: s & c
`-p PID`: check security context for process with this PID
`-f file_loc`: check security context for this file 

### user security context
```bash
semanage login -l #mapping of user to SELinux user
semanage user -l #see SELinux user and their role
semanage user -a selinux_username #add SELinux user
semanage login -a -s selinx_username loginID #map loginID to selinux_username
```
`man semanage` to see more information

### manage file security context
![[manage file security context.png| 600]]

### Manage policy package
Some tools fro managing policy rule package:
![[manage policy package.png  | 600]]

### Manage SELinux via Boolean
see list of all current booleans:
```bash
getsebool -a
getsebool httpd_can_connect_ftp
```
set bools with:
```bash
setsebool -P allow_user_exec-content off
```
the `-P` means permanent.

### logging
Make sure `auditd`, `rsyslogd` and `setroubleshootd` service are active.
```bash
aureport | grep AVC #see how many denial
ausearch -m avc #see in detail
journalctl | grep AVC #using journalctl

sealert -a /var/log/audit/audit.log #break down the issue
```

