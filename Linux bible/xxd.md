# xxd
well known for hexdumps or even the reverse.  Knowing this command thoroughly will help you handling hex strings and hex digits

## options
- `-b`: will give binary representation instead of hexdump
- `-E`: Change the character encoding in the right hand column from ASCII to EBCDIC (Feel free to leave this flag if you don't know about BCD notation)
- `-c int`: Sets the number of bytes to be represented in one row. (i.e. setting the column size in bytes; Default to 16) 
- `-g`: This flag is to set how many bytes/octets should be in a group i.e. separated by a whitespace (default to 2 bytes; Set -g0 if no space is needed).
- `-i`: To output the hexdump in C include format ('0xff' integers)
- `-l`: Specify the length of output(if the string is bigger than the length specified, hex of the rest of the string will not be printed)
- `-p`: Second most used flag; Converts the string passed into plain hexdump style(continuous string of hex bytes)
- `-r`: Most used flag, will revert the hexdump to binary(Interpreted as plain text).
- `-u`: Use uppercase hex letters(default is lower case)
- `-s`: seek at offset (will discuss this in a little brief in examples)