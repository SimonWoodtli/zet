# VirtualBox setup headless VM for Windows host

As an alternative to WSL2 setting up a Linux server VM is faster and just as 
convenient to use. The idea here is to keep the VM headless without the regular 
GUI that VMs come with. This will use a lot less resources. And connected via 
SSH it will be super fast.

* with VMs you can mount devices which you can't with WSL2

## Part 1: Install software and setup VM

1. Download [VirtualBox]
1. Download [Ubuntu Server]
1. Launch VirtualBox hit 'New' and go through the Installer

Preferred settings:

* name: ubuntu (or whatever name you wanna give it to ssh)
* 2GB Ram
* 2 Cores
* VDI (Virtual Disk Image), dynamically allocated
* 100GB HDD (ideally)

After the initial setup before you launch/start the VM go to networking and 
change it to bridged depending on which method you want to use 
(see. part 2 networking). You can also leave it as the default (NAT) and 
create an SSH tunnel to reverse ssh into it.

Learning to create an ssh tunnel and automate a reverse ssh back to your host
is also needed if your machines are connected to a VPN. So it's good to know
how to do that.

## Part 2: Install Linux on new VM

1. Start VM - workspace
1. Select your downloaded server ISO: Ubuntu, openSUSE, Rocky Linux, Alma Linux
1. Go through the installation process 

### Networking

Default networking settings are NAT (shared IP with Host machine). There are 
two methods to setup your workspace. 

1. default networking with NAT and reverse SSH
2. bridged networking and directly SSH

***Advantage NAT***

* no security related configuration and installation needed -> because NAT only
your computer can see and access the VM
* no extra work -> secure server
* safe but weak point is that your host machine needs to open ssh port 22 for 
reversed ssh

***Advantage Bridged***

* your host machine remains client only and doesn't need to open port 22 for 
reversed ssh

#### Networking Installation

Decide whether you want DHCP or static IP -> since I want to SSH into the VM
static IP is needed.
* Option 1: Setup static IP within the VM install process
* Option 2: use default settings and use your routers DHCP to give your VM a 
static IP (this requires a bridged VM, otherwise your router can't see VM)
* Proxy (only needed if you are at work)

***Warning*** if the IP in Option 2 shows a 10.x.x.x range you forgot to change 
NAT to bridged

I choose option 2 so I don't need to deal with reverse SSH and stuff.

[Reverse SSH]:

1. After install and reboot of server figure out your host machines local ip `ip a`
1. open ssh port 22 on your host machine
1. Login to your VM and ssh into your host machine `ssh HostMachineIP`
1. Figure out the rest of the process hint: `ssh -r VM-IP`

### HDD

* don't encrypt HDD just slows system down
* LVM Group is wanted
* always choose entire disk, don't use custom allocation
* default settings for file system and partition is fine

### Name

* server name = hostname (needed to ssh)
* make sure to use same username as your username on your host machine is

### SSH and additional software

* install OpenSSH server without ssh keys
* keep it minimal and skip rest of software
* download and setup docker manually
* install (takes some time) reboot (takes forever)

## Part 3: Configure your freshly installed server VM

NOTE: ~/.ssh/authorized_keys is just a list of public ssh-keys

1. login with credentials and update `sudo apt update && upgrade`
1. check your VMs ip: `ip a` and write it down somewhere can't copy it :(
1. use `exit` to log out
1. on your host machine: `ssh VMIP` make sure to have the same username on VM 
as on your host system otherwise `ssh VM-username@VMIP` needed.
1. make sure your host machine has a ssh-key otherwise: `ssh-keygen -t ed255191`
and then `ssh-copy-id hostIP`  so you don't have to login with pw. 
1. `ssh VMIP` and `sudo su -` and `visudo` to give yourself root access without
pw add under %sudo ALL... `yourUSERNAME ALL=(ALL:ALL) NOPASSWD: ALL`
advantage: still logs all your sudo commands but no pw needed
1. add the VM to your .sshconfig so you don't need to type the IP every time
`touch ~/.ssh/config` and add this:

```
Host ubuntu
  HostName 192.168.0.128
  User [yourUserName]
```

no pw sudo advantage: still logs all your sudo commands but no pw needed

```
Host VMname
  Hostname VMIP
  USER yourUsername
  IdentityFile //use path to old ssh-key or make a new one
```

If you run an openSSH server on windows you can also ssh into your windows 
machine and get powershell. That's cool :)

1. login to your router and give the current IP as reserved to your VM. It's 
not a static IP but good enough.

#### Start VM headless

1. shut down your VM `shutdown -h now`
on windows with vmware use `vmrun.exe`
to start: `vmrun.exe start C:...path-to-VM --headless`

Since I used virtual box:
to get your VMs name: `VBoxManage.exe list vms`
create new command `newx yourVMname`

The command on linux is the same without .exe

```
#!/bin/sh
exec VBoxManage.exe startvm --type headless yourVMsName
```

to start it: `yourVMname` and `ssh yourVMname`

#### Config Server

1. `mkdir -p Repos/github.com/SimonWoodtli`
1. create a keypair and add it to github `ssh-keygen -t ed25519`
1. clone your dotfiles repo and setup stuff
1. `scp` your ~/config/gh/host file to your VM so you don't have to authenticate
gh again...
1. disable password access to your vm via ssh so only if you have ssh-key you
get accesss `sudo vi /etc/ssh/sshd_config` and change `PasswordAuthentication no`
and `PermitRootLogin no`
1. disable root acces to your vm: `sudo vi /etc/passwd` change root:x...:/bin/bash 
to ...:/usr/sbin/nologin
1. if you want your usb with the private repo mounted on your VM and git be able 
to push from ~/Private to ~/usb/private.git you need to have an ssh-keypair on 
your vm. And then add your pub key to ~/.ssh/authorized_keys so git push works
1. set your date: `sudo timedatectl set-timezone America/New_York`

[Reverse SSH]: <https://www.howtogeek.com/428413/what-is-reverse-ssh-tunneling-and-how-to-use-it/>
[VirtualBox]: <https://www.virtualbox.org/>
[Ubuntu Server]: <https://ubuntu.com/#download>

Tags:

    #virtualMachine #headless #wslReplacement #linux
