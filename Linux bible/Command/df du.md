command to see space consumption on your system disk

## df
df show disk space available in linux and their mount point. Useful for seeing the mount point of removable disk(flashdisk).
```bash
df -h /dev/sdb1
df -h /mnt/mymusic
df
```
The first one will show where `/dev/sdb1` is mounted. While the second one show what device is mounted at `/mnt/mymusic`. df only show all available space.
### options
- `-h`: show human readable output
- `-t type`: print only filesystem with particular `type`
- `-x type`: exclude filesystem with this `type`
- `-a`: include filesystem with no space
- `--block-size=number` display disk space in certain block size

## du
see how much space is consumed by a particular directory.
```bash
du
du /home/jake
du -h /home/jake
```
`du` show space consumed by pwd. `-s` see total disk space used.