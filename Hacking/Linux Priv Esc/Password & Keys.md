### History files
Sometimes, user input a password in the console as an argument, so it would be saved in a history file.
Open history file with
```bash
cat ~/.*history | less
```
Probably search for an argument that looks like a password

### Config FIles
config files sometimes contain a plaintext user/password credentials. In this one the file `myvpn.ovpn` reference a file that contain the user and password.

### SSH Keys
Sometimes a wild ssh keys text is given a improper permission, in this case a ssh key in the root folder is readable by all.
Copy the ssh key to your attacker machine then use is to login.
Use `chmod 600 root_key` because ssh won't accept with improper permission.
```bash
ssh -i root_key root@10.10.180.76
```

