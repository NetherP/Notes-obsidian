Weak /etc/passwd
```bash
ls -l /etc/passwd
	-rw-r--rw- 1 root root 1009 Aug 25  2019 /etc/passwd
```

Same as [[writable shadow]], you can edit the /etc/passwd file, replace with a new password created with
```bash
openssl passwd newpasswordhere
```

or you can create a new root user by copying the root block and change the name to anything(newroot in this example)