# Share Folder from host to VM with KVM/Qemu Virtmanager

## On Host

1. Create a Folder on host that you want to share
1. Change permisssion: `chmod folder 777` (not sure if this is still necessary)
1. Open VM in Virtmanager and go to 'Details' and 'Add Hardware' 'Filesystem'

Driver: virtiofs
Source Path: Place/where/shared/DIR/on/Host
Target Path: Name of Virtiofs within the VM, e.g. 'shared'

## On Guest/VM

1. By default the Guest already created a dir for mounting the virtiofs in /mnt/xxx (xxx being the name you gave it on the host, e.g. 'shared')
1. Temporary mount FS: `sudo mount -t virtiofs shared /mnt/shared` (shared being the name given on the host)
1. Permanent mount FS, add to /etc/fstab: `shared           /mnt/shared       virtiofs    defaults        0       0`

> 🧐 On Windows VMs this process is slightly more complicated see Link below

Related:

* https://forum.manjaro.org/t/how-to-setting-up-shared-folders-auto-resize-vm-and-clipboard-share-with-virt-manger/127490
* https://discussion.fedoraproject.org/t/how-to-map-a-linux-host-dir-to-a-win11-guest-with-qemu/79760

Tags:

    #linux #sysadmin #virtualmachine #VM #KVM #virtmanager #QEMU #FS #filesystem
