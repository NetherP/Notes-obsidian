Some writable folder sometimes can be abused for PATH privesc.
Get all writable directory with:
```bash
find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u
```