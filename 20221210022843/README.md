# Setup Proxmox on Raspberry Pi 4 with Pimox

> 🧐 Only do this if you have a 4GB or better yet 8GB model. The other models simply won't have enough ram to deal with the overhead of running Proxmox and a VM.

## 1. Flash Raspberry Pi OS Lite

1. Startup your OS of choice
1. Plugin your SSD
1. open Raspberry Pi Imager choose 'Raspberry Pi OS Lite 64bit'
1. enable ssh and setup username/pw
1. flash OS

## 2. Install Pimox

> 🧐 Normally Proxmox runs of a lightweight Debian install. However Pimox is running of your Raspberry Pi Lite OS.

1. connect Pi via Ethernet
1. bootup Pi with SSD connected
1. ssh into pi: `ssh username@piIP`
1. install pimox: `sudo su -c "bash <(curl -s https://raw.githubusercontent.com/pimox/pimox7/master/RPiOS64-IA-Install.sh)" root`

> ⚠️  Pimox needs a static IP. However ssh into it on root@statIP prompts a pw that did not work for me. My guess is that the Raspbery Pi OS doesn't ship with a root pw and the Pimox install script doesn't change that either. However I was able to ssh into Pimox with the regular user that I setup during the flashing Raspberry Pi OS Lite. Then switched to root `su -` and added my public ssh key to /root/.ssh/authorized_keys.

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
    1. Networks: Here the default settings should be fine. Make sure a 'vmbr0' (bridged adapter) is selected so your VM can directly communicate with other devices on your LAN. 

    If you can't see 'vmbr0' you need to manually create a bridge first. Go to 'your-named-lroxmox-node' directly under the 'Datacenter' and then 'Network' 'Create'. Select 'Linux Bridge' your settings shoud be: 'IPV4/CIDR' is the static IP of your Proxmox Server, 'Gateway' is your Routers IP  and 'Bridge ports' is 'eth0' (Ethernet Network Interface (should be created automatically by Proxmox otherwise you need to create that too)
    1. finish up the and create VM
1. once the VM is created  click on the 'VM ID' that you gave it before
    1. Hardware 'Remove': delete the CD-ROM. 
    1. Hardware 'Add': add a 'CD-ROM'. Choose 'SCSI' because 'IDE' is not supported by the arm kernel. Then 'storage' 'local' and pick your VM iso that you downloaded in 'ISO Image'.
    1. Options 'Bootorder': put your new SCSI cd-rom to the top
1. startup VM and install OS
1. after install Shutdown machine. And change the Bootorder back again

### First Bootup of VM

> 🧐 Since we used a bridged adapter for the Network settings you should be able to ssh into your VM from any computer on your LAN.

1. boot up your VM 
1. ssh into it
1. update system: `sudo apt update && apt upgrade`
1. install guest agent: `sudo apt install qemu-guest-agent`
1. create a static IP:  (you can also ommit these steps and setup a static IP from your Routers Settings)
1. `cat /etc/netplan/00-installer-config.yaml`

Change this:

```
network:
  ethernets:
    enp0s18:
      dhcp4: true
  version: 2
```

To that:

```
network:
  renderer: networkd
  ethernets:
    ## leave this name as it was before
    enp0s18:
      ## turn off DHCP
      #dhcp4: true
      addresses:
        ## your new static IP:
        - 192.168.8.49/24
    ## if you don't have a DNS use a public DNS. E.g. Cloudflare
      #nameservers:
        #addresses: [1.1.1.1 1.0.0.1]
      routes:
        - to: default
          # your router IP address:
          via: 192.168.8.1
  version: 2
```

7. apply netplan: `sudo netplan apply`
8. ssh into VM with again with new IP
9. check changes: `ip a` and `ip r s`
10. turn off VM: `shutdown now`
11. Go to Proxmox webinterface 'VM id' and 'Options' then 'QEMU Guest Agent' and enable it
12. Boot up your VM
13. ssh into Pimox/Proxmox Host and test if qemu works `qm agent <vmid> ping` if no errors it's working

### What works

* USB passthrough but set it to USB2, USB3 will crash
* QEMU Guest Agent
* Backup
* ssh into Proxmox/Pimox Host
* ssh into VMs

> 🧐 Pimox works with other SBCs that are based on arm too.

### What doesn't work

* Snapshots

[Pimox]: <https://github.com/pimox/pimox7>
[ubuntu server]: <https://ubuntu.com/download/server/arm>

Related:

* <https://networkchuck.com/vmware-raspberrypi/>
* <https://pve.proxmox.com/wiki/Qemu-guest-agent>

Tags:

    #linux #raspberryPi #sbc #virtualMachine #proxmox #hypervisor #type1
