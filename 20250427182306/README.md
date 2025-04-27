# Dietpi with SSD Boot

1. Flash dietpi with rpi imager onto sd card
1. Ssh into it with root and do the initial config steps
1. Reboot and login again
1. Change to 'latest' not 'default' eeprom firmware: `vim /etc/default/rpi-eeprom-update`
1. Reboot and login again
1. Check if you are on latest eeprom: `rpi-eeprom-update` check service: `systemctl status rpi-eeprom-update`

Only continue with next steps if you managed to get latest eeprom running!

1. Format SSD with exFat and plug it into your pi
1. Turn off services and swap: `dietpi-services stop; swapoff -a`
1. Find your ssd with `lsblk` and copy data: `dd if=/dev/mmcblk0 of=/dev/sda bs=1M status=progress` (maybe you got different disk names double check yours)
1. Wait about 30mins to finish
1. Turn down pi: `shutdown -h now`
1. Unplug power cable and remove sd card then plug the power back in

Extend capacity:
1. Install `apt install cloud-guest-utils`
1. Resize Partition: `growpart /dev/sda 2` (means /dev/sda2, weird syntax)
1. Resize Filesystem: `resize2fs /dev/sda2`
1. Verify: `lsblk`

Related:

* <https://dietpi.com/forum/t/firmware-bootloader-update-raspberry-pi-4/5426/5>

Tags:

    #linux #rpi #dietpi #homelab
