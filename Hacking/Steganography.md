extracting file from image. Search for file signature https://en.wikipedia.org/wiki/List_of_file_signatures. Using hex editor like `xxd`
```bash
xxd cutie.png
```
If you find the signature, separate it with `dd` by offsetting with the location of the hidden file
```bash
dd if=cutie.png bs=1 skip=34562 of=secret.zip
```
where if= is the compund file, bs=1 means blocksize=1bytes, skip by 34562 bytes, and of= is the output file.


Checking with exiftool, there is sometimes a warning:
```bash
exiftool cutie.png
```


OR EASIER to just use binwalk

```bash
binwalk cutie.png
```
it will show all file hidden.

The jpeg file can't be viewed with binwalk. But it can be decoded by steghide
```bash
steghide info cute-alien.jpg # see the info
steghide extract -sf cute-alien.jpg

```

use `stegseek` to crack steghide file, can also use  stegcracker but its slower

## steghide
stego tools for hiding file in jpg.
```bash
steghide embed -ef FILE_TO_HIDE -cf COVER_FILE -p PASSWORD #hide file
steghide extract -sf FILE_TO_EXTRACT #extract
```

## zsteg
ruby app, a png version of steghide.
```bash
gem install zsteg #install ruby app

zsteg pngfile.png
```

## exiftool
look at metadata, can also uncover hidden files sometimes.

## stegoveritas
More comprehensive tools for steganography.
```bash
pip3 install stegoveritas # install from pip
stegoveritas_install_deps # install dependency


```
