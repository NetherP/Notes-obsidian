```bash
find filename
find /pathtofind filename
find /etc
find $HOME -ls
```
`find` run by itself search below the pwd.

Can produce lots of error with insufficient permission, use 
`2> /dev/null` to throw away error message.



#### options
- `-ls`: long listing(ownership, permission, size, etc)
- `-name filename`: search for filename
- `-iname`: same as name but ignoring case
- `-size`: search for file with size exacty, larger(+), or smaller(-) example:
```bash
find /usr/share/ -size +10M 
find /mostlybig -size -1M 
find /bigdata -size +500M -size -5G -exec du -sh {} \;
```
last one search for more than 500MB and less than 5GB
- `-user`: search by owner. use `-group` for group. `-or` and `-not` can be used to refine the search ex:
```bash
find /home -user chris -ls
find /home \( -user chris -or -user joe \) -ls
find /etc -group ntp -ls
find /var/spool -not -user root -ls
find / -type f -newermt 2013-09-12 ! -newermt 2013-09-14
```
- `-perm`: search by permission. Like chmod, use three digit number `777`. `-` used before number to match file that have that permission turned on.`/` for any permission by that number. Without both, the permission must match the number.
- `-type`: `f` for files, `d` for directories.
- `-(a/c/m)time (+/-)number`: `a` for access, `c` for change, and `m` for metadata to search for anything based on days of `number`. `-`for x ago, `+` for from x days ago to older.
- `-(a/c/m)min`: same as time but for minutes instead of days.
- `-newer(a/c/m)t`: used with `yyyy-dd-mm` to exclude file with access/modified/change date before specified date, use `!` before the option to exclude file after the date
- `-or` and `-and`: can also be used for other arguments.
- `-exec command {} \;`: run command for each file found. The {} is the substitution point for the founded file. `\;` escape the semicolon.
- `-ok`: same as `-exec` but prompt for each file that is found.
- `-ls`: display the result with the `ls -dils` output
