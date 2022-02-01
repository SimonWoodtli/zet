
# Arch Install


\# set keyboard layout - default is US

1. Set font & time

```sh
pacman -Sy terminus-font
setfont ter-v28n
timedatectl set-ntp on
```

2. verify boot mode (UEFI or Legacy):

$ `ls /sys/firmware/efi/efivars`
\# If the directory does not exist, the system may be booted in Legacy BIOS Mode

3. check internet connection / disk

```sh
ping archlinux.org
lsblk -f
```

4. Sync repositories, Install Reflector and Create mirrorlist, & install latest keyring

```sh
pacman -S archlinux-keyring
pacman -S reflector
reflector --verbose  --protocol https --latest 10 --sort rate --save /etc/pacman.d/mirrorlist
```

5. delete & create partitions

$ `gdisk /dev/sda`

steps:
d -> delete
n -> new partition (leave defaults except last sector: +512M) also use ef00 to create efi system on your boot partition
L -> list and then select ef00
p -> check if all is good
n -> all at default (should be second partition which will be the root filesystem (Linux filesystem)
p -> check if all is good
w -> write to disk

6. format  /boot filesystem

$ `mkfs.vfat -F 32 /dev/sda1`

7. create encrypted root

$ `cryptsetup -y -v luksFormat /dev/sda2`

8. open encrypted root

$ `cryptsetup open /dev/sda2 cryptroot`

9. format root filesystem

$ `mkfs.ext4 /dev/mapper/cryptroot`

10. mount root

$ `mount /dev/mapper/cryptroot /mnt`

11. mkdir for /boot & mount it

```sh
cd /mnt
mkdir boot
mount /dev/sda1 boot
```

12. use pacstrap to load stuff into root filesystem (base = base arch system, linux = kernel)

$ `pacstrap /mnt base base-devel linux linux-firmware linux-headers sudo vim`



13. create filetable

$ `genfstab -U /mnt /mnt/etc/fstab`

14. jump into your Arch environment

$ `arch-chroot /mnt`

15. create user and give sudo access:

```sh
useradd -m -G sero wheel -s /bin/bash username
passwd username
export VISUAL=vim
export EDITOR=vim
```

$ `visudo`
# uncomment line: # wheel is admin group in arch
%wheel ALL=(ALL) ALL


16. configure locale:

$ `vim /etc/locale.gen`
# uncomment line: en_US.UTF-8 UTF-8

$ `locale-gen`
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8 # make it available during install process

17. set time:

```sh
timedatectl set-ntp on
timedatectl list-timezones # pick yours
ln -sf /usr/share/zoneinfo/America/Buenos_Aires /etc/localtime
hwclock --systohc
```

18. configure networking

\# A: choose your own hostname (computer name)
$ `echo skynet > /etc/hostname`

\# B: hosts file
$ `vim /etc/hosts`

Add lines:

```sh
127.0.0.1	localhost
::1		localhost
127.0.1.1	skynet
```

19. edit hooks so root filesystem gets decrypted in init bootup:


$ `vim /etc/mkinitcpio.conf`
\# search for the HOOKS line: 1. replace udev with systemd 2. after filesystems add: sd-encrypt
\# HOOKS=(base systemd autodetect modconf block filesystems sd-encrypt keyboard fsck)

$ `mkinitcpio -p linux` \# run hooks again , standard kernel
OR:
$ `mkinitcpio -p linux-lts` \# with LTS kernel

20.A) install bootctl # instead of using the bootctl bootloader I could use grub

```sh
bootctl install
cd /boot/loader/entries
touch arch.conf
vim arch.conf
```

\# content: # !!blkid (in vim to get the UUID of your root filesystem)

```sh
title	Arch Linux
linux	/vmlinuz-linux
initrd	/initramfs-linux.img
options rw rd.luks.name=<HERE Goes UUID of /sda2 rootfilesystem>=cryptroot root=/dev/mapper/cryptroot
```

\## edit loader.conf
$ `vim /boot/loader/loader.conf`
\## content: (use tab once for space)

```sh
timeout 5
console-mode keep
editor	no
default	arch.conf
```

## update settings:

```sh
bootctl update
bootctl list # to check everything is good
```

20.B) use grub bootmanager

```sh
pacman -S grub efibootmgr os-prober
mkdir /boot/EFI
```

\## watchout you already may have mounted it step 11. (umount first if that is the case)

```sh
mount /dev/sda1 /boot/EFI
grub-install --target=x86_64-efi  --bootloader-id=grub_uefi --recheck
grub-mkconfig -o /boot/grub/grub.cfg
```


21.A install gnome GUI \#optional

```sh
pacman -S xorg
pacman -S gnome gnome-tweaks gdm
```

21.B) install dwm GUI (without display manager), \#optional

```
pacman -S xorg xorg-xinit nitrogen alacritty picom
mkdir -p /home/sero/Repos/github.com && cd $_
git clone https://aur.archlinux.org/paru.git && cd $_
makepkg -si
paru -S librewolf dwm-distrotube-git dmenu-distrotube-git nerd-fonts-mononoki nerd-fonts-ubuntu-mono
```

\## also install dwm and stuff (make your own dwm/dmenu builds!)
\## consider using https://xmonad.org/ instead of dwm! but written in haskell ... ;o

```sh
mkdir -p /home/sero/Repos/gitlab.com/xnasero && cd $_
git clone https://gitlab.com/xnasero/dotfiles.git && cd $_
./setup
```

21.C) checkout https://gitlab.com/dwt1/dotfiles/-/tree/master/.xmonad


22. enable daemons

```sh
pacman -S networkmanager
systemctl enable NetworkManager # internet when logging in
systemctl enable gdm # gnome display manager, or any display manager you want to use
```

\# systemctl enable dhcpcd.service ## only if u want dhcpcd instead of networkmanager

23. exit chroot

```sh
exit
umount -a
reboot
```
