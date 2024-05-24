# dd cheatsheet

dd is a common UNIX-based program whose primary purpose is the low-level
copying and conversion of raw data. It is used to copy a specified number of
bytes or blocks, performing on-the-fly byte order conversions, as well as being
able to convert data from one form to another. It can also be used to copy
regions of raw device files, for example backing up the boot sector of a hard
disk, or to read fixed amounts of data from special files like /dev/zero or
/dev/random. 

The basic syntax is: `dd if=input-file of=output-file options`

* Create a file with the following command: `dd if=/dev/zero of=outfile bs=1M count=10`
* Back up an entire hard drive to another by running: `dd if=/dev/sda of=/dev/sdb`
* Create an image of a hard disk with: `dd if=/dev/sda of=sdadisk.img`
* Back up a partition with this command: `dd if=/dev/sda1 of=partition1.img`
* Back up a CD ROM by doing: `dd if=/dev/cdrom of=tgsservice.iso bs=2048`
* Fill up with random data (good for encrypted usb-sticks): `dd if=/dev/urandom of=/dev/sdd1/foo bs=1M` (if quick use /dev/zero)

Tags:

    #linux #cheatsheet #terminal #hdd #device #backup
