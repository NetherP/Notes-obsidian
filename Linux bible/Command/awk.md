# awk
Another strings operators like [[tr]]
awk is powerful, but can be slower than [[sed]] or [[xargs]]
It is basically a scripting language for manipulating strings.

Syntax: `awk [flags] [select pattern/find(sort)/commands] [input file]`

## options
- `-f`: used to specify a script from a file like `awk -f script.awk input.txt`
- `-F`: specify the FIELD SEPARATOR, thus don't need to use BEGIN
- `-v`: can be used to specify variable(i don't understand this, check man pages)
- `-D`: to debug awk script like: `awk -D script.awk`
- `-o`: specify output file

## pattern
pattern is specified in a single quote.
- `/pattern/` can be used to grep only record(line) matching the pattern
- `'{print}'`: used by itself to print a file to output
```bash
awk '{print}' file.txt 
awk '{print $1,$3}' file.txt #print field 1 and 3 
#comma used to 
```
- `BEGIN'`: specify some rules like the FS to apply for the following pattern
```bash
awk 'BEGIN{FS="o"} {print $2}' file.txt #set the delimiter to 'o' for the following pattern
awk 'BEGIN{RS="o"} {print $0}' file.txt #create new row where delimiter 'o' is.
awk 'BEGIN{OFS=":"} {print $1,$2,$3}' file.txt #separate the field with ':' instead of space(default)
awk 'BEGIN{ORS="\n\n"} {print $0}' file.txt #separate the record with double newline instead of single newline(default)
awk 'BEGIN{FS=" "; OFS=":"} {print $1,$4}' awk.txt #use ; to use more than one code
```

## variables
Some built in variables:
 - `$1 $2 $3 $#`: Field variables, specify data separated by delimiter(space by default). `$0` is the whole line.
 - `NR`: Number Record, basically the line number.
 - `FS`: Field Separator, used to specify the delimiter to determine field variable. usually used in BEGIN to specify what the delimiter used for
 - `RS`: Record Separator. Create a new row(default to line) replacing the argument used. Example `{RS="o"}` will create new row in place where 'o' is.
 - `OFS`: Output Field Separator. Set the separator to be used, replacing the default separator(space when using comma $1,$2,etc) with the argument
 - `ORS`: Output Record Separator. Set the separator for record(row), replacing the default row separator(newline `\n`)


## Resources
-   [AWK - Workflow - Tutorialspoint](https://www.tutorialspoint.com/awk/awk_workflow.htm) (For learning awk scripting in brief and quick)
-   [The printf statement in awk](http://osr5doc.xinuos.com/en/OSUserG/_The_printf_statement.html) (If you want to do more with formatting strings; you can use printf function also)  
-   [AWK command in Unix/Linux with examples - GeeksforGeeks](https://www.geeksforgeeks.org/awk-command-unixlinux-examples/)
-   And if you really want to dive deep on this tool, do check out man pages on gawk