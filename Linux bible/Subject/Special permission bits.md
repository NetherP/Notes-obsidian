This is SUID, SGID, and sticky bit.

The `chmod` actually takes 4 number as argument(example `chmod 644` is actually `0644`). The first number is special permission with:
 - `4`(`u+s`): SUID
 - `2`(`g+s`): SGID
 - `1`(`o+t`): Sticky bit
 
 If set on command, command with SUID and SGID set will run on the user owner and group owner respectively.
 
 If set on directory, any files created in SGID set directory will be assigned to the directory owner's group.
 
 Directory with sticky bit set will become restricted deletion directory.
 In restricted deletion directory, only the owner and root can delete the file, even if other user have the write permission.