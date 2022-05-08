```bash
grep term /file
grep desktop /etc/services
grep -i desktop /etc/services
grep -vi tcp /etc/services
grep -rli peerdns /usr/share/doc/
```
`grep` search for search term in file or in a folder recursively.


### options
- `-i`: case insensitive
- `-v`: exclude lines with term
- `-r`: recursive search. the location must be a folder `/location/`
- `-R`: same as -r but also follow symbolic links
- `-h`: don't show file prefix for recursive search
- `-l`: just list the file that contain the term in `-r` search.
- `-c`: only show how many times the pattern is found
- `-n`: show line numbers
- `-e`: can be used to specify multiple pattern, like `-e PATTERN1 -e PATTERN2` etc. It works with BRE/normal grep
- `--color`: highlight the term in the line
- `command| grep term`: to use grep for standard output for `command`. 

### grep, egrep, fgrep
`grep` use basic regular expression which means it only recognize some regex if its escaped with backslash(`\`)
`egrep` or `grep -E` use regular expression which means it use all regex special character and treat special character as string only with backslash
`fgrep` or `grep -f`use fixed string, which means it doesn't recognize regex whether it use backslash or not.


## Cheat sheet
```bash
grep '^....$' #grep for 4 character words, adjust dot number
grep -E '^[[:alpha:]]{4}$' #grep for 4 letter words. change 4 to any number
cat * | grep -Eo "([0-9]{1,3}[\.]){3}[0-9]{1,3}" #search for an ip address in file in this folder, and just output the result(-o)
```