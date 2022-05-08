check capabilities with:
```bash
getcap -r / 2>/dev/null
```

binaries with `cap_setuid`  sometimes can be used as a privilege escalation vector. Check gtfobins