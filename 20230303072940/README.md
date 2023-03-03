# NixOS install

## Format Disk

We will format with 4 partitions:

1. boot
2. OS
3. homedir
4. swap

If your HDD is > 2TB use gdisk/parted with GPT. If less use MBR with fdisk

### Gdisk

Main hotkeys:

1. help: `?`
1. delete partition: `d`
1. delete all partitions and create GPT: `o`
1. new partition: `n`
1. write: `w`
1. name/label partition: `c`

Steps:

1. find dev: `lsblk`
1. start cmd: `sudo gdisk /dev/sdXX`
1. `o` and `w`
1. restart cmd and `n`



### Parted

Here we use UEFI BIOS so starts from 512MiB, If legacy BIOS: 1MiB

1. create partition table: `sudo parted /dev/sdXX -- mklabel gpt`
1. create NixOS partition: `sudo parted /dev/sdXX -- mkpart primary 512MiB -1340GiB)` -xxxGiB is substracted from the end of the HDD. Since I want to create a swap and another homedir partition I need some space.



