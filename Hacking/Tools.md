# Tools
List tools/website that's used for cybersecurity
## attackerkb.com
a forum for exploit to grade a CVE value and exploitability to attacker. Search here for a CVE on whether it's easy to use/valuable. 
AKB explorer can be used to search AKB like searchsploit look at exploitdb.
https://github.com/horshark/akb-explorer

## hashcat-utils
https://hashcat.net/wiki/doku.php?id=hashcat_utils
located in /usr/share/hashcat-utils in kali. Containing tools useful for cracking password/hash.
### rli
Compare a single file against another and remove duplicate from the first file
usage: `rli infile outfile removefiles...`
infile: file that you want to erase the duplicate from
outfile: the result
removefiles: file/s that you want to compare duplicate against

## md5hashing.net
decode/decrypt hash

## wpscan
tools for scanning a wordpress site. Can be used to detect vulnerability, enumeration, and bruteforcing.
```bash
# bruteforcing for user(-U) c0ldd,philip,hugo
wpscan --url http://10.10.141.86/ -U c0ldd,philip,hugo -P /usr/share/wordlists/rockyou.txt --password-attack wp-login -t 32


```