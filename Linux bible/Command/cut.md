extract fields from line of text or files.
```bash
cut -d'delimiter' -f1 input
grep /home /etc/passwd | cut -d':' -f6 -
```

### options
- -d'delimiter': choose the delimiter
- -f#: # is the field number that you want to extract
- `-`: this is to replace the input to the standard input(pipe as in the second case).