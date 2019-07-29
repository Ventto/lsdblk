LSDBLK
======

*"List all the data partitions of a given block device"*

# Usage

* List all the `/dev/sda`'s blocks:

```bash
$ lsblk -o NAME,FSTYPE /dev/sda
NAME          FSTYPE
sda
├─sda1        vfat
└─sda2        crypto_LUKS
  └─lvm       LVM2_member
    ├─vg-swap swap
    ├─vg-boot ext4
    ├─vg-root ext4
    └─vg-home ext4
```

* List the `/dev/sda`'s data blocks:

```bash
$ lsdblk /dev/sda
/dev/mapper/vg-boot ext4
/dev/mapper/vg-home ext4
/dev/mapper/vg-root ext4
/dev/sda1 vfat
```
