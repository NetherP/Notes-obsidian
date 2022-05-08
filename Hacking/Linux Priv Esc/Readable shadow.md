weak /etc/shadow permission
weak password
crackable by john

hashes = `$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0` (sha512crypt)

```bash
john --wordlist=/home/kali/rockyou.txt ./hash.txt                                                                   
	Using default input encoding: UTF-8
	Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
	Cost 1 (iteration count) is 5000 for all loaded hashes
	Will run 2 OpenMP threads
	Press 'q' or Ctrl-C to abort, almost any other key for status
	password123      (?)
	1g 0:00:00:18 DONE (2021-10-28 03:49) 0.05274g/s 81.01p/s 81.01c/s 81.01C/s cuties..mexico1
	Use the "--show" option to display all of the cracked passwords reliably
	Session completed
