# tr
`tr` (Translate) is the most basic of string operation it use 2 set of character, SET1 or source and SET2 or target. Sets can be regex charset `[A-Z]` or predefined sets that can be viewed in `--help`

## options
 - `-d`: To delete a given set of characters, use 1 set or source only
 - `-t`: truncate, or concat where target + source
 - `-s`: Replace source with target, the default options
 - `-c`: complementer(reverse the flag)

 ## example
```bash
tr source target

#example
FOO="Mixed UPpEr aNd LoWeR cAsE"
echo $FOO | tr [A-Z] [a-z]
```
The example change the input text($FOO) uppercase letter[A-Z] into lowercase [a-z].

```bash
for file in * ; do 
	f=`echo $file | tr [:blank:] [_]`
	[ "$file" = "$f" ] || mv -i -- "$file" "$f" 
done
```
This one replace all filename(file) in current folder that have blank space(:blank:) with underscore(_).
