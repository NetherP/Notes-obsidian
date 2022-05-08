sort the file or if no file, read from standard input(pipe into sort)

```bash
sort filename
sort -rn filename
du | sort -rn 
```

### options
- `-r`: reverse the result
- `-c`: check whether its already sorted
- `-n`: sort by numerical value(number)
- `-u`: same as piping to uniq
- `-o save.txt`: save output