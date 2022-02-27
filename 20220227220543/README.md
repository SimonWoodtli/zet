# Partition USB-Stick with encryption

## Programs needed

fdisk, gdisk, cryptsetup

## Create partitions and format them:

1. figure out where your stick is mounted with `lsblk` or `mount`
1. use fdisk if MBR: `fdisk /dev/sdX` or for GPT: `gdisk /dev/sdX`
1. in the terminal app create partitions (n) and change their type (t)
1. when done use (w) to write/safe
1. use: `mkfs.fat /dev/fsdX` or: `mkfs.ext4 /dev/sdX2` to create the file system
or `mkfs.foo /dev/foo` for another file system

## Create encrypted partition

1. for security write some random data on the partition you want to encrypt:
`dd bs=4K if=/dev/urandom of=/dev/sdX2`
1. encrypt your partition: `cryptsetup -h sha256 -c aes-xts-plain -s 256 luksFormat /dev/sdX2`

## Label and Mount encrypted partition

Note that for regular partitions you name/label them directly. For example if 
it's a fat partition: `exfatlabel /dev/sdX1 yourLabelName` (see related). But an 
encrypted partition requires the systems device mapper to name the partition.

1. Use this command to name/label: `cryptsetup luksOpen /dev/sdX1 yourLabelName`
1. Now your partition is available to you under `/dev/mapper/yourLabelName`
1. create a mount point: `mkdir -p ~/mnt/usb`
1. mount partition: `mount /dev/mapper/yourLabelName ~/mnt/usb`
1. if you don't mount your partiton in your own home you need to change owner:
`chown -R myusername.myusername /mnt/usb`

## Unmount encrypted partition

1. `umount /mnt/usb`
1. `cryptsetup luksClose /dev/mapper/yourLabelName`

Related:

* create another zettel how to label/name your partitions
* create another zettel how to auto-mount usb stick on a specific place
* create another zettel how to store encryption passphrase (maybe not needed)
* <https://www.tecmint.com/change-modify-linux-disk-partition-label-names/>

Tags:

    #linux #partition #fileSystem #usbStick #format #hardDisk #sysadmin
