## parted
parted is more modern tools, can work with gpt.
`parted -l` or `fdisk -l` show all disk and it's partition.

```bash
parted /dev/sdb
```
open parted command and use `/dev/sdb`.
Some of its command:
- `p`: show all disk & partition
- `rm number`: remove partition number, used withouth number for prompt.
- `mklabel gpt`: create the partition table(mbr, gpt, loop).
- `mkpart`: create partition, will be prompted it's name, filesystem, and location(size, begining & end).

This is not inside parted, but you need to create filesystem with mkfs:
`mkfs -t xfs /dev/sdb1`: create xfs filesystem for `/dev/sdb1`
`mkswap /dev/sdb2`: make `/dev/sdb2` a swap filesystem.
`pvcreate /dev/sdb6`: make `/dev/sdb6` a LVM physical volume

## fdisk
fdisk is older and can't work with gpt, only mbr.

```bash
fdisk /dev/sdb
```
check the help menu(`m`) to see what commands are available.
