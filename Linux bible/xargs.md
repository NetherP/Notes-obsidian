# xargs
another string operators (STROPS). xargs works with a command's argument, manipulating it before being used as an argument for some command.

## options
- `-0`: Will terminate the arguments with null character (helps to handle spaces in the argument)
- `-a file`: This option allows xargs to read item from a file
- `-d delimiter`: To specify the delimiter to be used when differentiating arguments in stdin
- `-L int`: Specifies max number non-blank inputs per command line
- `-s int`: Consider this as a buffer size that you allocate while running xargs, it sets the max-chars for the command, which includes it's initial arguments and terminating nulls as well.(You won't be using this most of the times but it's good to know). Default size is around 128kB (if not specified).
- `-x`: This flag will exit the command execution if the size specified is exceeded.(For security purposes.)
- `-E str`: This is to specify the end-of-file string (You can use this in case you are reading arguments from a file)
- `-I str`: (Capital i) Used to replace str occurrence in arguments with the one passed via stdin(More like creating a variable to use later)
- `-p`: prompt the user before running any command as a token of confirmation.
- `-r`: If the standard input is blank (i.e. no arguments passed) then it won't run the command.
- `-n int`: This specifies the limit of max-args to be taken from command input at once. After the max-args limit is reached, it will pass the rest arguments into a new command line with the same flags issued to the previously ran command. (More like a looping) 
- `-t`: verbose; (Print the command before running it).Note: This won't ask for a prompt

## examples
```bash
# create variable from stdin to be used as argument for sh -c
echo "file1 file2 file3" | xargs -t -I argVar sh -c '{ touch argvar; ls -l argVar; }'

# find file then separate the result with null bytes so it can work with spaces in the arguments, then pass it to rm -f
find /home/pjrox/Desktop/ -type f -print0 | xargs -0 -t -p rm -f

# create files with names from a file
cat file | xargs -I files -t sh -c '{ touch files; chmod 400 files; }'

# create a file of list of files in current directory, then delete the files
ls | xargs -I word -n 1 -t sh -c 'echo word >> shortrockyou; rm word'
```