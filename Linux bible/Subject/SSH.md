# SSH
## Type
Secure shell type:
ssh: secure shell, remote shell application
scp: secure copy
sftp: secure ftp

### Package:
openssh, openssh-clients, opnessh-package

### Starting
To determine sshd status:
```bash
#RHEL 6
service --list sshd

#FEDORA
systemctl status sshd.service

#UBUNTU
systemctl status ssh.service
```

To start:
```bash
#RHEL 6
service sshd start

#FEDORA
systemctl start sshd.service

#UBUNTU
systemctl start ssh.service
```
This method only start the service withouth setting it to start automatically at boot

To start at boot:
```bash
#RHEL 6
chkconfig sshd on

#FEDORA
systemctl enable sshd.service

#UBUNTU
systemctl enable ssh.service
```

### config
config for the daemon: `/etc/ssh/sshd_config`

## ssh
login to remote system running sshd service.
```bash
#normal login
ssh johndoe@10.140.67.23

#run command
ssh johndoe@10.140.67.23 command_name
ssh johndoe@10.140.67.23 "cat myfile"
#command with graphical interface
ssh -X johndoe@10.140.67.23 system-config-printer
ssh -X johndoe@10.140.67.23 	#login with X application enabled
```

public key of remote host(registered the first time you connect) is added to `~/.ssh/known_hosts` for asymmetric encryption. Delete the entry if there is error because the server changes key. The main config is in `/etc/ssh/sshd_config`

## scp
copy host-to-host with encryption
```bash
scp source destination
scp /home/chris/memo johndoe@10.140.67.23:/tmp

#recursive(directory's content)
scp -r johndoe@10.140.67.23:/usr/share/man/man1/ /tmp/
```

## rsync
Copy file like `scp` but specifically used for backup
```bash
rsync -avl johndoe@10.140.67.23:/usr/share/man/man1/ /tmp/
rsync -avl --delete johndoe@10.140.67.23:/usr/share/man/man1 /tmp
```
### Options
- `-a`: recursive
- `-v`: verbose
- `-l`: copy symbolic links
- `--delete`: mirror the directory(remove file that doesn't exist in source)

## sftp
Doesn't actually use ftp, and instead use sshd to mimic ftp style of interaction.
```bash
sftp johndoe@jd.example.com
```

## ssd using keys
1. Generate key with: `ssh-keygen`. identification (`~/.ssh/id_rsa` by default) is the private key while `~/.ssh/id_rsa.pub` is the public key.
2. copy the public key to the remote host with `ssh-copy-id -i ~/.ssh/id_rsa.pub johndoe@10.140.67.23`
3. Check the remote host `$HOME/.ssh/authorized_keys` for the recently added public key. Alternatively, step 2 can be skipped by copying the key directly to this folder.