## Shell escape sequence
`sudo -l` to see what program can be run as sudo for this user

[GTFOBins](https://gtfobins.github.io/) : compendium for unix program that can be used as post exploit vulnerability

Follow the sudo part of the website to get root privilege

## Environment Variables
`sudo -l` also show inherited environment variable from the user that can be injected with root shell


## !root permission
if user can't run as only root, you can bypass it by running
```bash
sudo -u#-1 command
sudo -u\#-1 command
```
which is the same as root. Can only works for some older version.
check version with `sudo --version`
More Detailed:
https://www.exploit-db.com/exploits/47502