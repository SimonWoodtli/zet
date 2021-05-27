# How to compare the checksum of a .iso image with the copied content on your CD-ROM?

The trick to verifying the media is to limit the calculation to only the portion of the optical media that contains the image. We do this by determining the number of 2,048-byte blocks the image contains (optical media is always written in 2,048-byte blocks) and reading that many blocks from the media. On some types of media, this is not required. CD-R and CD-RW disks are written in disc-at-once mode can be checked this way.

Many types of media, such as DVDs, require a precise calculation of the number of
blocks. In the example below, we check the integrity of the image file dvd-image.iso
and the disc in the DVD reader /dev/dvd.

`$ md5sum dvd-image.iso; dd if=/dev/dvd bs=2048 count=$(( $(stat -c "%s" dvd-image.iso) / 2048 )) | md5sum`
