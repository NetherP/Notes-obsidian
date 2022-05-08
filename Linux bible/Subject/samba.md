samba is based on windows file and printer sharing, SMB protocol. It can connect to windows file and printer protocol as if it is a native windows.

### Install
```bash
yum install samba samba-client samba-winbind
```
`samba` is the samba server package.
`samba-client` is the package for connecting to samba as a client, including the `smbclient`, `nmblookup`, and `findsmb` command.
`samba-winbind` is the package that lets you connect your linux machine to becoma a member of windows domain.

### config files
`/etc/samba/smb.conf` is the main conf files.
`/etc/samba/lmhost` enable Samba NetBIOS hostname to be mapped into ip address.

You can create `/etc/samba/smbusers` to map linux usernames into windows usernames.

`man smb.conf` for more information

### Starting and Stopping
There is two daemon:
- smb: Daemon that provides file and prnter sharing service.
- nmb: providing NetBIOS name service.

```bash
systemctl start smb.service #systemd
systemctl stop smb.service
systemctl start nmb.service
systemctl stop nmb.service
service smb start #init
service smb stop
service nmb start
service nmb stop
```

### create and access samba
create user with `smbpasswd -a chris`. You will be prompted for new password. This can only work for users inside your machine.

You can access chris(or any user) home directory with:
`smbclient -U chris //192.168.0.119/chris` substituting for the server ip and the username.
You need to enable home dirs if SELinux is in enforcing mode with
`setsebool -P samba_enable_home_dirs on`, -P is permanent

### Creating and adding shared folder
Create shared folder with this commands:
```bash
mkdir /var/salesdata #this is the shared folder
chmod 775 /var/salesdata 
chown chris:chris /var/salesdata # in this case the file can only be modified by chris but visible by everyone
semanage fcontext -a -t samba_share_t /var/salesdata #create selinux rules
restorecon -v /var/salesdata #apply selinux rules
touch /var/salesdata/test #testing the permission
ls -lZ /var/salesdata/test #see the permission and SELinux file context
```

Add shared folder by creating a container like this:
```smb.conf
[salesdata]
	comment = Sales data for current year
	path = /var/salesdata
	read only = no
	browseable = yes
	valid users = chris
```
valid users allow those user to access the shared folder. User can be added to make comma separated list.
### Access
Access it with smbclient:
```bash
smbclient -L localhost -U chris #checking the SAMBA/listing
smbclient -U chris //localhost/salesdata #enter salesdata with username chris
```
