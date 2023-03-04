# NixOS install

## 1. Format Disk

We will format with 4 partitions:

1. boot
2. OS
3. documents/data 
4. swap

If your HDD is > 2TB use gdisk/parted with GPT. If less use MBR with fdisk


### 1.1 Create Partition Table and Partitions

Parted:

> 🧐 Here we use UEFI so the first nixos partition starts from 512MiB, If legacy BIOS: 1MiB

cmd explained: `parted -- mkpart partition-label partition-type startpoint[MiB|%] endpoint[MiB|%]`  
The Order of executing parted mkpart cmds matters, as the partition number starts with 1 and goes to 4 in this case.

1. create partition table: `parted /dev/sdXX -- mklabel gpt`
1. If UEFI, create boot partition: `parted /dev/sdXX -- mkpart ESP fat32 1MiB 512MiB`
1. create NixOS partition: `parted /dev/sdXX -- mkpart primary ext4 512MiB 27%` , if legacy BIOS it'd be 1MiB instead of 512MiB
1. create Document/Data partition: `parted /dev/sdXX -- mkpart secondary ext4 27% 99%`
1. create Swap partition: `parted /dev/sdXX -- mkpart swap linux-swap 99% 100%`

If UEFI, set esp flag to on : `parted /dev/sdXX -- set 1 esp on`, 1 refers to first partition
If BIOS, set boot flag to on: `parted /dev/sdXX -- set 1 boot on`

Gdisk:

I prefer `parted` as fdisk/gdisk is a TUI and writing documentation for instructions is annoying. But those main hotkeys are only needed to get the same result as with `parted`.

* to start TUI: `gdisk /dev/sdXX`

Main hotkeys:

* help: `?`
* delete partition: `d`
* delete all partitions and create GPT: `o`
* new partition: `n`
* write: `w`
* name/label partition: `c`


### 1.2 Create Filesystems for each Partition:

1. If UEFI, boot partition: `mkfs.fat -F 32 -n boot /dev/sdXX1`
1. swap partition: `mkswap -L swap /dev/sdXX4`

**Optional**: Only run the next commands to create a file system for your primary/secondary partitions if you do not want to encrypt them.

1. primary partition: `mkfs.ext4 -L nixos /dev/sdXX2`
1. secondary partition: `mkfs.ext4 -L data /dev/sdXX3`

### 1.3 Encrypt primary/secondary Partitions:

> 🧐 If you use the same luks password for multiple disks, partitions and want them to autostart at boot time, you only need to enter it for one disk/partition

1. encrypt primary: `cryptsetup -y -v luksFormat --label crypto-nixos /dev/sdXX2`
1. encrypt secondary: `cryptsetup -y -v luksFormat --label crypto-data /dev/sdXX3`
1. decrypt primary: `cryptsetup open /dev/sdXX2 primary`, primary is just the name to mount the container later
1. decrypt secondary: `cryptsetup open /dev/sdXX3 secondary`
1. create fs primary: `mkfs.ext4 -L nixos /dev/mapper/primary`
1. create fs secondary: `mkfs.ext4 -L data /dev/mapper/secondary`

## 2. Mount partitions

1. create dir: `mkdir /mnt`, mount: `mount /dev/disk/by-label/nixos /mnt`
1. If UEFI, create dir: `mkdir /mnt/boot/efi`, mount: `mount /dev/disk/by-label/boot /mnt/boot/efi`
1. If BIOS, create dir: `mkdir /mnt/boot`, mount: `mount /dev/disk/by-label/boot /mnt/boot`
1. create dir: `mkdir -p /mnt/home/xnasero/data`, mount: `mount /dev/disk/by-label/data /mnt/home/xnasero/data`

## 3. Initial NixOS Configuration

1. generate config files: `nixos-generate-config --root /mnt`
    1. build/customize config files in /mnt/etc/nixos/*
    1. or rm auto generated config files and curl my preconfigured files: `curl ...`

## 4. Install OS

> 🧐 If you want to make changes to your config files after the first install don't use `nixos-install` instead use `nixos-rebuild switch` or `nixos-rebuild build`

1. `nixos-install`, give a root pw at the end of install
2. reboot into your new OS 🎉
