file transfer protocol, insecure, all transfer is in plaintext, including the authentication process.

vsftp is the most popular ftp server in linux.


### Starting
```bash
sevice vsftpd start #init
systemctl start vsftpd.service #systemd
```

### Config
The main conf files is `/etc/vsftpd/vsftpd.conf`.
Example of conf file can be found in `/usr/share/doc/vsftpd` directory.

The example conf file `/usr/share/doc/vsftpd/EXAMPLE/INTERNET_SITE/vsftpd.conf` can be used to open your fpt server to the internet. 
Be sure to backup your original `vsftpd.conf` file beforehand.

### client
`lftp`