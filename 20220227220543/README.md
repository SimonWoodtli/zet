# Partition USB-Stick with encryption

## Programs needed

fdisk, gdisk, cryptsetup

## Create partitions and format them:

1. figure out where your stick is mounted with `lsblk` or `mount`
1. use fdisk if MBR: `fdisk /dev/sdX` or for GPT: `gdisk /dev/sdX`
1. in the terminal app create partitions (n) and change their type (t)
1. when done use (w) to write/safe
1. create a file system on all every partition: `mkfs.fat /dev/sdX1` and 
`mkfs.ext4 /dev/sdX2`  

## Create encrypted partition

1. for security write some random data on the partition you want to encrypt:
`dd bs=4K if=/dev/urandom of=/dev/sdX2`
1. encrypt your partition: `cryptsetup -y -v luksFormat /dev/sdX2`

## Label and Mount encrypted partition

Note that for regular partitions you name/label them directly. For example if 
it's a fat partition: `exfatlabel /dev/sdX1 yourLabelName` (see related). But an 
encrypted partition requires the systems device mapper to name the partition.

1. Use this command to name/label: `cryptsetup open /dev/sdX2 yourLabelName`
1. Now your partition is available to you under `/dev/mapper/yourLabelName`
1. make a file system on it: `mkfs.ext4 /dev/mapper/yourLabelName`
1. add this to fstab: UUID is from the nested filesystem 

to get all UUIDs: `lsblk -f /dev/sdX`


```bash
UUID=92c1cf2c-ae92-41c5-8c93-69fa998f78af /home/sero/mnt/usb ext4 defaults,nls=utf8,umask=000,dmask=027,fmask=137,uid=1000,gid=1000 0 0
UUID=0C3741977176D897 /home/sero/mnt/USB ntfs-3g defaults,nls=utf8,umask=000,dmask=027,fmask=137,uid=1000,gid=1000 0 0
```

1. add this to crypttab: UUID is from the LUKS wrapper/container   
IMPORTANT: A Luks partition has two UUIDs one for the container and one for the 
actual file system.

```bash
usb UUID=c0b8c577-95b7-4567-9fce-6e5a1c631a49 none nofail
```

1. create a mount point: `mkdir -p ~/mnt/usb ~/mnt/USB`
1. mount partition: The first mount should be done from GUI so you can
store the passphrase. 

> Note: `mkfs.foo /dev/foo` for another file system  
> Note: It's also possible to do all from the CLI but then you need more 
parameters in /etc/crypttab and create a file in /etc/luks-keys/yourLabelName 
where you want to store your passphrase.  
> Note: Normally the ext4 partitions need only a defaults parameter in /etc/fstab
but for some reason my usb-stick was always getting root dir ownership.

## Unmount encrypted partition

1. `umount /mnt/usb`
1. `cryptsetup luksClose /dev/mapper/yourLabelName`


Related:

* [20220228043945](/20220228043945/) NTFS and Fat partitions on Linux 🐧
* [20220302215119](/20220302215119/) Label/Name your USB-sticks/HDDs partitions
* create another zettel how to store encryption passphrase (maybe not needed)
* <https://www.tecmint.com/change-modify-linux-disk-partition-label-names/>

Tags:

    #linux #partition #fileSystem #usbStick #format #hardDisk #sysadmin
