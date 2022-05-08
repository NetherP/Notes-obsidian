# curl
Crawl URL. Accessing the web with command line.

## options
- `-#`: Will display a progress meter for you to know how much the download has progressed.(or use --silent flag for a silent crawl)
- `-o`: Saves the file downloaded with the name given following the flag.
- `-O`: Saves the file with the name it was saved on the server.
- `-C -`: This flag can resume your broken download without specifying an offset.
- `--limit-rate`: Limits the download/upload rate to somewhere near the specified range (Units in 100K,100M,100G)
- `-u`: Provides user authentication (Format: -u user:password)
- `-T`: Helps in uploading the file to some server(In our case php-reverse-shell)
- `-x`: If you have to view the page through a PROXY. You can specify the proxy server with this flag. (-x proxy.server.com -u user:password(Authentication for proxy server))
- `-I`: (Caps i) Queries the header and not the webpage.
- `-A`: You can specify user agent to make request to the server
- `-L`: Tells curl to follow redirects
- `-b`: Allows you to specify cookies while making a curl request(Cookie should be in the format "NAME1=VALUE1;NAME2=VALUE2")
- `-d`: POST data to the server(generally used for posting form data).
- `-X`: To specify the HTTP method on the URL. (GET,POST,TRACE,OPTIONS)

## Resources
-   [curl command in Linux with Examples - GeeksforGeeks](https://www.geeksforgeeks.org/curl-command-in-linux-with-examples/)
-   [curl - How To Use](https://curl.se/docs/manpage.html)
-   [15 Tips On How to Use 'Curl' Command in Linux (tecmint.com)](https://www.tecmint.com/linux-curl-command-examples/)
