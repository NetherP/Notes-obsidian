An audit daemon to monitor your file. It monitor by keeping tracks of the files inode number(like a hash).

```bash
auditctl -w /etc/passwd -p rwa
auditctl -W /etc/passwd -p rwa(s)
```

- `-w`: input filename to monitor
- `-p`: The permission to monitor, r=read w=write x=execute a=attribute change.
- `-W`: stop monitoring this file with the -p argument
- `-l`: see all the audit and their setting


To review the audit logs, use
```bash
ausearch -f /etc/passwd
```

the `-f` options look at the logs by the file argument.