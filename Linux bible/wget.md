# wget
short of web-get.

## options
- `-b`: To background the downloading process
- `-c`: To continue to the partially downloaded file (It will look for the partially downloaded file in the directory and starts appending; takes no argument
- `-t int`: To specify retries to the URL
- `-O download.txt`: To specify the output name of downloaded file
- `-o file`: To overwrite the logs into another file
- `-a file`: To append the logs into already existing file without deleting previous contents 
- `-i file` Read the list of URLs from a file.
- `--user=username`: To give a login username(Use --ftp-user and --http-user if doesn't work)
- `--password=password`: To give a login password( Use --ftp-password and --http-password if doesn't work)
- `--ask-password`: Ask for a password prompt if a login is necessary. (I recommend using this flag instead of --password because there are chances that password might start with $ or something else that can be interpreted as something else in your terminal) 
- `--limit-rate=10k`: Similarly to curl(supports k and m notation for kB and mB respectively)
- `-w=<int>`: This is to specify the waiting time before the retrieval from a URL.(Takes time in seconds)
- `-T=<int>`: Timeout the retrieval after a specified amount of time.(Takes time in seconds)
- `-N`: Enables timestamping 
- `-U`: To specify the user-agent while downloading the file