# how to add disks to fstab for autostart?

if the disk is not encrypted: add this line at the end of the /etc/fstab file
/dev/sdb  /home/sero/2nd-HDD  ext4  defaults  0  1


if the disk uses LUKS encryption:

1. mount the disk manuallf from the file explorer and type the passphrase and click the save button
2. `sudo vi /etc/fstab` and add this line:
/dev/mapper/secret      /home/sero/2ndHDD                 ext4    defaults        0 0
3. `sudo vi /etc/crypttab` and add this line at end:
secret  /dev/sdb       none
