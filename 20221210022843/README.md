# Setup Proxmox on Raspberry Pi 4 with Pimox

> 🧐 Only do this if you have a 4GB or better yet 8GB model. The other models simply won't have enough ram to deal with the overhead of running Proxmox and a VM.

## 1. Flash Raspberry Pi OS Lite

1. connect Pi via Ethernet
1. open Raspberry Pi Imager choose 'Raspberry Pi OS Lite 64bit'
1. enable ssh and setup username/pw
1. flash OS

## 2. Install Pimox

> 🧐 Normally Proxmox runs of a lightweight Debian install. However Pimox is running of your Raspberry Pi Lite OS.

1. bootup pi with ssd connected
1. ssh into pi: `ssh username@piIP`
1. install pimox: `sudo su -c "bash <(curl -s https://raw.githubusercontent.com/pimox/pimox7/master/RPiOS64-IA-Install.sh)" root`

> 🧐 Pimox needs a static IP. However ssh into it on root@statIP prompts a pw that did not work for me. My guess is that the Raspbery Pi OS doesn't ship with a root pw and the Pimox install script doesn't change that either. However I was able to ssh into Pimox with the regular user that I setup during the flashing Raspberry Pi OS Lite. Then switched to root `su -` and added my public ssh key to /root/.ssh/authorized_keys.

## Configure Proxmox with a VM

1. login to proxmox web interface on your browser: `https://theIPyouSET:8006`
1. go to shell and change swap size: `vi /etc/dphys-swapfile` change to 'CONF_SWAPSIZE=2048' \#don't use zram to keep as much cpu resources as possible
1. download arm64 [ubunut server]: goto 'local' 'Iso Images' 'Download from URL' give it a name and download it
1. go to 'Create VM'
    1. General: set 'VM ID' to a number
    1. General: set 'Name' to a name for your VM
    1. OS: set it to 'don't use any media'
    1. System: set 'BIOS' to 'OVMF (UEFI)'
    1. System: set 'EFI Storage' to 'local'
    1. Disks: Keep it as SCSI and set the size to whatever you want
    1. CPU: 'Socket' '1', 'Core' '2'
    1. Memory: depends on a 4GB model I wouldn't go over 2048
    1. Networks: keep it standard
    1. finish up the and create VM
1. Once the VM is created  click on the 'VM ID' that you gave it before
    1. Hardware 'Remove': delete the CD-ROM. 
    1. Hardware 'Add': add a 'CD-ROM'. Choose 'SCSI' because 'IDE' is not supported by the arm kernel. Then 'storage' 'local' and pick your VM iso that you downloaded in 'ISO Image'.
    1. Options 'Bootorder': put your new SCSI cd-rom to the top
1. Startup VM and install OS

### What works

* USB passthrough but set it to USB2, USB3 will crash
* QEMU Guest Agent
* Backup
* ssh into Proxmox

> 🧐 Pimox works with other SBCs that are based on arm too.

### What doesn't work

* Snapshots

### TODO

* figure out how to directly ssh into a VM

[Pimox]: <https://github.com/pimox/pimox7>
[ubuntu server]: <https://ubuntu.com/download/server/arm>

Related:

* <https://networkchuck.com/vmware-raspberrypi/>

Tags:

    #linux #raspberryPi #sbc #virtualMachine #proxmox #hypervisor #type1
