Gathering information withouth engaging directly with the target. Passive recon is like driving through the block to stake out the target house, in contrast active recon is like knocking the door checking who is home, or checking the windows if its locked.

OSINT usually used for passive recon

## Some tools
### whois
Querying the domain registrar some information about the domain. like who register it, what their contact number, what nameserver they use.
Usually pretty useless nowadays to get the admin info, as you need to enquire to get the contact.
```bash
whois tryhackme.com
```
### nslookup
Query a nameserver for an entry, default to A type entry(IPv4)
```bash
nslookup DOMAIN_NAME
nslookup google.com
nslookup OPTIONS DOMAIN_NAME SERVER
nslookup -type=A tryhackme.com 1.1.1.1
```
SERVER is a public dns server that you asked the information from.

Some type:
- A = IPv4 address
- AAAA = IPv6 address
- CNAME = canonical name
- MX = Mail server
- SOA = Starts of authority
- TXT = TXT records.

### dig
Like nslookup, but more detailed
```bash
dig DOMAIN_NAME
dig DOMAIN_NAME TYPE
dig @SERVER DOMAIN_NAME TYPE
dig @1.1.1.1 tryhackme.com MX
```
### dnsdumpster.com
This website list some subdomain that sometimes can't be found on search engine.

### host
query dns server for ip address that resolves to that domain
```bash
host clinic.thmredteam.com
```

### viewdns.info
website that can do reverse ip lookup. Useful to know what other domain that is registered on those ip.

### threatintelligenceplatform.com
website that collect info similiar to dig and whois and more, and present it in more readable way.

### censys.io
provide lots of information about IP address and domains.

### shodan
the cli is easier to use maybe
https://cli.shodan.io/

### peoplefinder.com
website for searching people in the US

### sublist3r
https://github.com/aboul3la/Sublist3r
enumerate subdomain by using search engine, available in kali. Can also use bruteforcing.

### hunter.io
list email related to a domain.

### builtwith.com
query various info for a domain(nameserver,hosting,etc).

### wappalyzer
query technology stack of a website, available as an addon for browser.
## advanced searching
- https://support.google.com/websearch/answer/2466433: google advanced search syntax
- https://help.duckduckgo.com/duckduckgo-help-pages/results/syntax/: duckduckgo search syntax
- https://help.bing.microsoft.com/apex/index/18/en-US/10002: bing search syntax
Some commonly used syntax for google
- `filetype:pdf`
- `site:blog.tryhackme.com`
- `-site:example.com`  : exclude site containing term
- `intitle:TryHackMe` : search for page with term in title
- `inurl:tryhackme`: page containing term in url

https://www.exploit-db.com/google-hacking-database contain collection of search term that can be useful

## Recon-ng
https://github.com/lanmaster53/recon-ng
frameworks that helps automate OSINT work. Some modules require API key to work, so check the page.

Open `recon-ng`
### creating a workspace
```bash
workspace create thmredteam #create thmredteam workspace

recon-ng -w thmredteam #start recon-ng with the specified workspace
```
### seeding the database
Organize the data collected in a database.
```bash
db schema #check name of the table in our database

db insert domains #insert the data into the domains table
```
### marketplace
get modules to transform the information that we have into other type of information in the marketplace.
```bash
marketplace search KEYWORD
marketplace info MODULE
marketplace install MODULE
marketplace remove MODULE
```
In search, the star (`*`) in the K column means that the modules required API keys while in the D column means it require third party library(dependencies).
### working with modules
```bash
modules search #search/list installed modules 
modules load MODULE #load specific MODULE to memory
#with modules loaded
options list
options set <option> <value>
run
```
### keys
```bash
keys list 
keys add KEY_NAME KEY_VALUE
kes remove KEY_NAME
```

## publik pgp key
You can import it to get the user/email of the key owner by importing it with:
`gpg --import PUBLICKEY`


## wiggle.net
search a wifi location/details from its SSID/BSSID