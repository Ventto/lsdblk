LSDBLK
======

*"List all the data partitions of a given block device"*

# Requirements

```
$ apt-get install util-linux
```

# Introduction

The *lsblk* tool lists all the `/dev/sda`'s block devices (as below) but
it cannot extract data ones you would like to clone with a tool like
*partclone*.

```bash
$ lsblk -o NAME,FSTYPE /dev/sda
NAME          FSTYPE
sda
├─sda1        vfat
└─sda2        crypto_LUKS
  └─lvm       LVM2_member
     ├─vg-swap  swap
    ├─vg-boot ext4
    ├─vg-root ext4
    └─vg-home ext4
```

Let's suppose we need to write a script to backup data partitions.
It is useful to list and iterate them while leveraging the `partclone.[fstype]`
tool suite:

```bash
for blk_info in $(lsdblk /dev/sda); then
    blk_name=$(echo "$blk_info" | cut -d' ' -f1)
    blk_fstype=$(echo "$blk_info" | cut -d' ' -f2)
    [ -z "$blk_fstype" ] && blk_fstype="dd"

    partclone.${blk_fstype} -c -s "$blk_name" | gzip -c -9 > "$blk_name".gz
fi
```

# Usage

* List the `/dev/sda`'s data blocks:

```bash
$ lsdblk /dev/sda
/dev/mapper/vg-boot ext4
/dev/mapper/vg-home ext4
/dev/mapper/vg-root ext4
/dev/sda1 vfat
```

* List the `/dev/sda1`'s data blocks:

```bash
$ lsdblk /dev/sda1
/dev/sda1 vfat
```

* List the `/dev/sda2`'s data blocks:

```bash
$ lsdblk /dev/sda2
/dev/mapper/vg-boot ext4
/dev/mapper/vg-home ext4
/dev/mapper/vg-root ext4
```
