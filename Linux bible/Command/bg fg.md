Run command in the background with ampersand(&)
```bash
find /usr > /tmp/allusrfiles &
```
The output of the program will appear even in bg, direct it to `2> /dev/null` to avoid it.

```bash
jobs
```
Show the command running in the background. `-l` to show the pid.

`Ctrl+Z` stop a command and put it in the background. 

```bash
fg %1
```
Bring jobs to foreground. Refer to the jobs by:
- %number: the job number
- %string: command that begin with string
- %?string: command that contain string
- %--: jobs stopped before the one most recently stopped(the - sign in jobs)

```bash
bg %5
```
Start the stopped jobs in the background.
