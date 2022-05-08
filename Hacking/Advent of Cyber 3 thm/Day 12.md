Talking about Network File System(NFS)
open port 2049(nfs) in nmap

use 
```bash
showmount -e target_ip
```
-e for showing exports list
List what file are being shared

mount some shared files with
```bash
mount target_ip:/shared_folder mountpoint

mount 10.10.253.253:/share mnt
```

