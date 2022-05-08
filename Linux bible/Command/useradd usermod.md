## useradd
Command to add and edit user accounts.
```bash
useradd username
useradd -c "Sara Green" sara
useradd -g users -G wheel,apache -s /bin/tcsh -c "Sara Green" sara
useradd -D -b /home/everyone -s /bin/tcsh #set default
```
default add user option is stored in `/etc/login.defs` and `/etc/default/useradd`

### Options
- `-c "comment"`: provide description/comment for the account.
- `-d home_dir`: the home directory. the default is /home/username.
- `-D`: save this command as the default instead of creating new account.
- `-e expire_date`: assign the expiration dateof the account.
- `-f number`: set number of days after the password expire to disable this account. `-1` is the default, which disable this options.
- `-g group`: set the primary group. New group with the username as the name will be created withouth this option.
- `-G grouplist`: add user to the supplied comma separated list of groups.`-aG` to add in usermod, as only`-G` would remove already assigned one.
- `-k skel_dir`: set skeleton directory, directory containing conf and script that would be moved to the new user home dir.
- `-m`: used with `-k`, basically use the skeleton directory.
- `-M`: Don't create new home directory.
- `-n`: only for Red Hat, disable default behaviour of creating new group that matches username and UID.
- `-o`: use with `-u uid` to make a user that has the same uid as another username, making a single account(permission) with multiple username.
- `-p passwd`: enter password. You can also use `passwd user` command later, or `openssl password` for encrypted MD5 pass.
- `-s shell`: specify the shell of the user.
- `-u uid` specify the uid of the user.


## usermod
command to edit existing user account.
```bash
usermod -s /bin/csh chris
usermod -Ga sales,marketing, chris
usermod -e 2021-01-01 tim
```

The option is almost the same as useradd except:
- `-l login_name`: change the login name
- `-L`: Lock the account, it put an `!` at  the beginning of the`/etc/shadow` password entry. `-U` to unlock.
- `-m`: used with `-d`(change home dir) to copy the current home dir to the new directory.

## userdel
Used to remove user account.
```bash
userdel -r chris
userdel chris
```
`-r` removes the user home directory as well.

Recommended to run `find / -user chris -ls` to search for all file belonging to chris(or deleted user) to clean up the file. 
After deleted, use `find / -nouser -ls` to find remaining files.

## chage
used to view and change account's password aging and also see account expiration date
```bash
chage -l tim | grep Account
chage -M 30 -m 5 -W 7 tim
chage -M 30 -I 5 tim
```

- `-M`: maximum days that password must be changed
- `-m`: set minimum days that passwod can be changed
- `-W`: set warning before being forced to change password
- `-I`: used with -M to set password expiry N days after the maximum day

## passwd
command to change your password, or add password to the current user.
```bash
passwd
passwd joe #run as root to change user password
```

You can eforce best password practice by adding an entry to `/etc/pam.d/common-password` or `/etc/pam.d/common-auth`.

You can also  set password expiration and length in `/etc/login.defs`.
