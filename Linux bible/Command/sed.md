# Stream editor (sed)
too complicated
```bash
sed -n '/home/p' /etc/passwd
sed 's/Mac/Linux/g' somefile.txt > fixed_file.txt
cat somefile.txt | sed 's/Mac/Linux/g' > fixed_file.txt
cat somefile.txt | sed 's/ *$//' > fixed_file.txt
```
It have the same syntax as in the vim ex mode.


Syntax: `sed [flags] [pattern/script] [input file]`
Where pattern/script is like this:
`'CONDITIONS MODES/SOURCE/TARGET/ARGS'`
```bash
sed -e '1,3 s/john/JOHN/g' file.txt
```
In this case conditions 1,3 is the range of line 1 to 3 and apply the code following to that range

## options
- `-e`: To add a script/command that needs to be executed with the pattern/script(on searching for pattern)
- `-f`: Specify the file containing string pattern
- `-E`: Use extended regular expressions
- `-n`: Suppress the automatic printing or pattern spacing
## modes/command
- `s`: substitue(find and replace) from SOURCE to TARGET
- `y`: rarely used, like s but works on individual bytes, no args

## args
- `g`: globally(any pattern change will be affected globally, i.e. throughout the text; generally works with s mode)
- `i`: To make the pattern search case-insensitive(can be combined with other flags)
- `d`: To delete the pattern found(Deletes the whole line; takes no parameter like conditions/modes/to-be-replaced string)
- `p`: prints the matching pattern(a duplicate will occur in output if not suppressed with -n flag.)
- `1 2 3 N`: To perform an operation on an nth occurrence in a line(works with s mode)
 You can combine number with g(1g 3g Ng) to apply for every Nth occurence in the line
## example
```bash
sed 's/  */ /g' file.txt #replace multiple space into single space

```

## resource
-   [Sed Command in Linux/Unix with examples - GeeksforGeeks](https://www.geeksforgeeks.org/sed-command-in-linux-unix-with-examples/)
-   [sed, a stream editor (gnu.org)](https://www.gnu.org/software/sed/manual/sed.html) (Official Documentation)
-   [15 Useful 'sed' Command Tips and Tricks for Daily Linux System Administration Tasks (tecmint.com)](https://www.tecmint.com/linux-sed-command-tips-tricks/)

