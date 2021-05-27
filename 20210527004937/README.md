# burn CD-ROM on CLI

1. Constructing an iso image file that is the exact file system image of the CD-ROM
`$ dd if=/dev/cdrom of=ubuntu.iso (for audio CDs use: cdrdao)`

1. Writing the image file onto the CD-ROM media
1.1 first create a dir with all the files you want to have in your image: `$ mkdir ~/cd-rom-files`
1.1 create diskimage from dir:
`$ genisoimage -o cd-rom.iso -R -J ~/cd-rom-files`

1.1 burn to cd/dvd
`$ wodim dev=/dev/cdrw image.iso`
