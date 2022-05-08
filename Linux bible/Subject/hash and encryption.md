Create a hash from a file with
```bash
sha256sum File_location
```


## Encryption
### gpg2
#### Resources
-   [gpg - Unix, Linux Command - Tutorialspoint](https://www.tutorialspoint.com/unix_commands/gpg.htm)
-   [GPG Cheat Sheet (hawaii.edu)](http://irtfweb.ifa.hawaii.edu/~lockhart/gpg/)

#### symmetric encryption
A symmetric key cryptography.
##### encrypting
```bash
gpg2 -c --force-mdc -o /tmp/backup.tar.gz.gpg /tmp/backup.tar.gz
gpg --cypher-algo AES-256 --symmetric secret.txt
```
`-c` to encrypt the file
`-o` is the output(/tmp/\*.gpg in this case)
`--cypher-algo cyphername'`set the cypher to cyphername
`--symmetric` set as symmetric encryption
##### decrypting
```bash
gpg2 -d --forcemdc /tmp/backup.tar.gz.gpg > /tmp/backup.tar.gz
gpg secret.txt.gpg
```
`-d` is used for decrypting

#### asymmetric encryption
Create the public/private key pair for the current user with
```bash
gpg2 --gen-key
```
This create a key ring in which the user can view all his keys.
```bash
gpg2 --list-key
```
The key can be used by extracting the key from the keyring with 
```bash
gpg2 --export John Doe > JohnDoe.pub
gpg2 --export johndoe@mail.com > JohnDoe.pub #can also use the email
```
where `John Doe` is the real name inputted when the key is generated(first command).
After which you can share the public key to be used.

The key can be used by others(jill in this case) by importing it to jill keyrings with:
```bash
gpg2 --import JohnDoe.pub
```
Then she can encrypt a file for John Doe with:
```bash
gpg2 --out MessageForJohn --recipient "John Doe" --encrypt MessageForJohn.txt
```
After the `MessageForJohn` file is received by john, he can decrypt it with his private key by:
```bash
gpg2 --out JillsMessage --decrypt MessageForjohn
```
`--out` for the output and `--decrypt` for the encrypted file

### Digital Signature
Used for proving the owner. Use private keys for encryption, the opposite of sending asymmetric encrypted file.

Example: User johndoe want to send a message to user christineb along with his digital signature. He use gpg with:
```bash
gpg2 --output JohnDoe.DS --sign MessageForJill.txt
```
This create a digital signature file(\*.DS) to that can be encrypted by jill with:
```bash
gpg2 --import JohnDoe.pub # if this is the first time jill got john public key
gpg2 --decrypt JohnDoe.DS
```
This method allow anyone with John public key to view the message. To make sending the message to jill secure, john can use jill public key with
```bash
gpg2 --sign --encrypt JohnDoe.DS
```
You will be prompted for the user ID of the recipient. Check in your keychain. or use `--recipient name/email` or `-r` instead.

## openssl
#### asymmetric file encryption
To generate a private key we use the following command (8912 creates the key 8912 bits long):
`openssl genrsa -aes256 -out private.key 8912`

To generate a public key we use our previously generated private key:
` openssl rsa -in private.key -pubout -out public.key`

Lets now encrypt a file (plaintext.txt) using our public key:
`openssl rsautl -encrypt -pubin -inkey public.key -in plaintext.txt -out encrypted.txt`

Now, if we use our private key, we can decrypt the file and get the original message:
`openssl rsautl -decrypt -inkey private.key -in encrypted.txt -out plaintext.txt`