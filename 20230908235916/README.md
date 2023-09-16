# Format HDDs with LUKS encryption and auto-mount

> 🧐 Giving Labels is extremly helpful if you plan to use your disks
with automounting feature on Linux. Both /etc/fstab and /etc/crypttab
can handle them. The biggest benefit as opposed to UUID of disks is you
can swap your disks connected SATA port. If you use UUID you cannot do
this as the UUID is different for each disk on each port.

## 1. Create Partition Table

> 🧐 You can also use sgdisk or sfdisk to do the partitioning (which I
prefer over TUIs like fdisk and gdisk)

1. If USB-Stick: `parted /dev/sdXX -- mklabel mbr`
1. If SSD/HDD: `parted /dev/sdXX -- mklabel gpt`

## 2. Create Partitions

> 🧐 There are two examples shown one fore single partition and one with
multi partition setup.


1. single partition: `parted /dev/sdXX -- mkpart nameOfPartition ext4 0% 100%`
1. multi partition0: `parted /dev/sdXX -- mkpart nameOfPartition0 fat32 1MiB 512MiB`
1. multi partition1: `parted /dev/sdXX -- mkpart nameOfPartition1 ext4 512MiB 27%`
1. multi partition2: `parted /dev/sdXX -- mkpart nameOfPartition2 ntfs 27% 100%`

## 3. Create Filesystem for none LUKS partitions

> ⚠  This Label is important and can be used for auto-mounting via /etc/fstab

> 🧐 Only do this step if you plan not to use LUKS. IF you do it it's
not the end of the world but creating a LUKS container would simply
overwrite it.

1. single FS: `mkfs.ext4 -L labelOfFS /dev/sdXX1`
1. multiFS0: `mkfs.fat -F 32 -n labelOfFS0 /dev/sdXX1`
1. multiFS1: `mkfs.ext4 -L labelOfFS1  /dev/sdXX2`
1. multiFS2: `mkfs.ntfs -L labelOfFS2 /dev/sdXX3`

## 4. Encrypting with LUKS

> 🧐 LUKS only really makes sense for Linux Filesystems like ext4.

### 4.1 Create LUKS Container: Encrypt

> ⚠ This Label is important as we need it later to mount via
/etc/crypttab

1. single FS: `cryptsetup -y -v luksFormat --label labelOfLUKSContainer /dev/sdXX1`
2. multi FS (only one has ext4): `cryptsetup -y -v luksFormat --label labelOfLUKSContainer /dev/sdXX2`

### 4.2 Decrypt the new LUKS container

1. single FS: `cryptsetup open /dev/sdXX1 container`
1. multi FS: `cryptsetup open /dev/sdXX2 container`

### 4.3 Create partition for LUKS container

> ⚠ This Label is important as we need it later to mount via /etc/fstab

1. single FS & multi FS: `mkfs.ext4 -L labelOfFS /dev/mapper/container`

## 5. Auto Mount disks using labels

If you like to thinker with your hardware you will be happy to plug your
disks on whichever port you want without having to manually adjust the
UUID everytime you do.

### 5.1 Mounting LUKS HDD

```
sudo sh -c 'printf "%s\n" "LABEL=2ndHDD /mnt/2ndHDD ext4 defaults 0 2" >> /etc/fstab'
sudo sh -c 'printf "%s\n" "2ndHDD LABEL=crypto-2ndHDD none" >> /etc/crypttab'
```

### 5.2 Mounting LUKS USB-Stick

### 5.3 Mounting HDD

### 5.4 Mounting USB-Stick

Related:

* [20220227220543](/20220227220543/) Partition USB-Stick with encryption
* [20220228043945](/20220228043945/) NTFS and Fat partitions on Linux 🐧
* [20220302215119](/20220302215119/) Label/Name your USB-sticks/HDDs partitions
