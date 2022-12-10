# SBC/PI OS Image: Create Backups or Clone your SD-Card

If you want a complete 1:1 backup of an SD Card with a flashed OS like DietPi, Raspian OS, Ubuntu Server etc. You can either create a compressed backup that you need to restore or clone your image on a new storage device.

## 1. Compressed Backup on your Computer

1. Plug your SD Card that has your OS on it into a Linux PC. If devices auto-mount make sure to `umount` the card first
1. check it's name: `lsblk`
1. copy image from your sd-card: `sudo dd if=/dev/sdb bs=1M status=progress | gzip > /home/your_username/dietpi_pi0_image-`date +%d%m%y`.gz`
1. restore image onto new sd-card: `gzip -dc /home/your_username/dietpi_pi0_image-xxx.gz | sudo dd of=/dev/sdb bs=1M status=progress`

Of course if you already use an USB-Stick or SSD to run your OS from you can use the same method to backup and restore the image.

## 2. Clone your SD card onto another SD card or SSD

This is only useful if you want to migrate from and old SD card to a newer one with more space. Or if you have a SBC that supports booting from USB-A you can also clone your OS image onto a SSD.

### 2.1 On Linux PC

1. Plug in your empty USB-Stick/SSD/SD-Card 
1. Plug in your SD-Card with the OS Image on it
1. check their names: `lsblk`
1. If they are auto-mounted `umount` them
1. create a clone if=source-image-os, of=to-be-cloned-storage: `sudo dd if=/dev/mmcblk0 of=/dev/sda bs=1M status=progress`

### 2.2 On your SBC

1. bootup you OS with via new USB-Stick/SSD/SD-Card
1. if your new storage device has more space than the old sd-card you need expand the available space. As the dd command created a 1:1 copy and even if you have more storage on your new device it won't let you access it unless you expand the device to the new maximal available space.
1. TODO HOW TO EXPAND?

Tags:

    #linux #sbc #pi #backup #clone #sd #sdd #hdd #usbStick #storage
