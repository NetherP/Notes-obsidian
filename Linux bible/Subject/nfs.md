Network File System. Used to share huge amount of data or disk space.

### Install
```bash
yum install nfs-utils
```

### Documentation
Exports man page: To configure NFS server, share your directories in `/etc/exports`.
`exportfs` man: describe how to share and view directories that shared from `/etc/exports`
`mount.nfs` man page: Describe how client can mount remote NFS directories.
`showmount` man page: Describe `showmount` command to see what directories are available from NFS server.

### Starting
```bash
systemctl start nfs-server.service #systemd
service rpcbind start
service nfs start #init
```
rpcbind service is needed for NFS, the systemd daemon automatically start rpcbind.

### Sharing NFS
Make an entry for directories that you want to share in `/etc/exports`.
Permission works by handling the same UID on client and server as the same user. There is also some options to map user.

Lastly, run `exportfs` command. 
```bash
exportfs -a -r -v
```

### Using NFS
```bash
showmount -e server.example.com #show shared directory
mount maple:/stuff /mnt/maple # serverhost:directory mountpoint

mount -t ntfs4 #show filesystems mounted from NFS file servers.
```
