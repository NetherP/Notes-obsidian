listing process

stat:
- S: sleep
- R: running
- +: foreground


### options
- -u/u : show username, cpu, mem, start, and stat.
- x: show all process(in contrast to show only process started by that terminal).
- a: show for all user
```bash
ps aux
```
- -e: list all running process, used with o
- -o argument: show the argument column, the options is:
	![[ps header.png]]
```bash
ps -eo "%p %y %x %c"
ps -eo pid,user,uid,group,gid,vsz,rss,comm
```
- --sort=(+/-)normal: replace normal with the above chart. + for ascending(optional) - for descending.
-  