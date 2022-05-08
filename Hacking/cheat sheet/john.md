script to convert file to be readable by john is located in /usr/share/john

## Example
```bash
#ssh private key
/usr/share/john/ssh2john.py id_rsa > john_id_rsa
john --wordlist=/usr/share/wordlists/rockyou.txt john_id_rsa 

#using wordlist
john --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-MD5 robot_pass 

#unshadow for /etc/passwd and /etc/shadow file
unshadow /etc/passwd /etc/shadow
#then use sha512crypt format

#single mode, using mangling and GECOS to bruteforce based on username
john --single --format=[format] [path to file]
```