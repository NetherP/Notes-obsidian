open with `#!/bin/bash` or execute in terminal with `bash script`.

```bash
MYDATE=$(date)
```
Assign `date` command output to `MYDATE` variable. So you can assign a command output to a variable with `$(command)` syntax.
Can also use backtick 
```md
`command`
```
to assign variable, but unlike `$()`, backtick is run once when its assigned.

`$0, $1, $2, $3, $n` is the argument of the script, where `$0` is the name of the script(with its full path). `$#` show number of arguments. `$@` shows all arguments. `$?` shows exit code for the last ran command.
```bash
read -p "this is the prompt text" param1 param2 param3
```
`read` is a prompt for inputting parameter.

use `let` before variable to use arithmetic, as in:
```bash
let life=70+2 #life is 72
```
`help let` to see more explanation.
