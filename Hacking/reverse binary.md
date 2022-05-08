# Reverse binary
## Tools
### Radare2
reverse engineer tools
[[radare2]]
Search some resource to learn about this
```bash
r2 -d EXECUTABLE ARGS1
```

### ltrace
list all dynamic library calls that is executed by the program and see what it's arguments. useful for reverse engineering file that uses c library
```bash
ltrace EXECUTABLE ARGS1
```