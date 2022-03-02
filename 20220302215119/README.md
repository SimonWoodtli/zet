# Label/Name your USB-sticks/HDDs partitions

* for ext2, ext3, ext4: `e2label /dev/sda1 <yourLabel>`
* for NTFS: `ntfslabel /dev/sda5 <yourLabel>`
* for SWAP: `mkswap -L <yourLabel> /dev/sda5`
* for exFAT: `exfatlabel /dev/sda3 <yourLabel>`

Tags:

    #linux #hdd #terminal #partitions #format #nameHDD #nameUSB #label
