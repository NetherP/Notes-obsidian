## mount
mount a device on the linux filesystem
```bash
mount device_location mount_location
mount /dev/sdb1 /mnt/test
```
the `mount` only command show all currently mounted filesystem
`-t` for filesystem
`-o` for options

You can mount image file(iso) with the command 
`mount -o loop image-location.iso /mnt/point`


## umount
unmount mounted device. Mount point is not needed in the argument, only mounted device.
```bash
umount /dev/sdb1
```
`-l`: lazy unmount, unmount only when the device is not busy anymore.
`-f`: force unmount.

You can see what's holding back umount with the `lsof /mnt/point` command.

## fstab
`/etc/fstab` is a file that show/set device that is mounted automatically on boot.
```bash
#example entry
/dev/sdb1	/mnt/test	xfs	defaults	0	1
```
This entry means that the device is `/dev/sdb1` mounted at `/mnt/test` as an `xfs` filesystem. `defaults` means that its mounted at boot, `0` means to not automatically backup with the `dump` command. `1` means check for error after a number of mount.
Check `man mount` in the `-o` section for the options that can be set in the fourth field.