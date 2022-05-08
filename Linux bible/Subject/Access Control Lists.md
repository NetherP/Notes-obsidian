Permission with ACL, enhanced permission in linux. 
ACL can be assigned only by the owner(user) of the file/directories.

## setting ACL
```bash
setfacl -m u:username:rwx filename
getfacl filename
```

`-m`  modify ACL permissions, while `-x` remove ACL permissions. 
`u:` for user and `g:` for group
`username` is the user you grant permissions to, `rwx` is the permission granted.  

`getfacl` see the ACL of the file/direcotry.
the mask line show the maximum permission that can be granted by ACL(set with the regular group permissions)

```bash
setfacl -m d:g:market:rwx /tmp/mary/
```
In this command, `d:` is used to set default ACLs on a directory, making file in that directory inherit the ACLs

## Enabling ACLs
ACL is enabled in the filesystem. ACL can be enabled in several way:
- Add `acl` option to the fifth field in the line in the `/etc/fstab` file. This file automatically mount filesystem when the system boot up
- Implant `acl` line in the Default mount options field in the filesystem super block.
- Add `acl` option to the `mount` command.

Check if acl option is enabled with:
```bash
tune2fs -l /dev/sdb | grep "mount options"
```
in this command, `/dev/sdb` is the name associated with the filesystem(check with`mount | grep home`).

Set the acl in default mount options with:
```bash
tune2fs -o acl /dev/sdc1
```

the third otion is like this:
```bash
mount -o acl /dev/sdc1 /var/stuff
```