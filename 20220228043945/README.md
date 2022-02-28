# NTFS and Fat partitions on Linux 🐧

## FAT

As far as I can tell from my research it is not possible to change permissions 
on a fat formatted partition.

* experiment: format your USB stick one more time to FAT and use the entry from
NTFS on /etc/fstab. (just got to adjust ntfs-3g to vfat) -> maybe it works? 😯

## NTFS

For NTFS just add this line to your /etc/fstab to not have new files with -x 
permissions.

```bash
UUID=EEA2B69CA2B668AB   /home/sero/mnt/USB    ntfs-3g   defaults,nls=utf8,umask=000,dmask=027,fmask=137,uid=1000,gid=1000,windows_names 0 0
```

Tags:

    #linux #partition #sysadmin #usbStick #usb #
