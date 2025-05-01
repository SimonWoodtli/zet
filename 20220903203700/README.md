# Proxmox Install Guide

Things you can install or have on your node:

* LXCs
* Container images/templates: You can safe your LXC container into an image then mess
  around with your LXC container and if things break. You can delete the LXC
  container and create a new LXC from the image. (Same concept to how docker
  container/images work)
* VMs

## 1. Install

1. DL iso onto Ventoy stick
1. Bootup proxmox and go through initial settings

```
user: root pw: *** mail: any mail you'd like to use
Domain: pve1.home.arpa (or anything you'd like)
IP: 192.168.1.99 (or any IP that is free on your router, I suggest to DHCP bind that IP so it gets reserverd as a static IP for your proxmox node)
Gateway: 192.168.1.254 (or whatever your router IP is)
DNS: 1.1.1.1 (or whatever DNS server you'd like to use)
```

## 2. Configure/Setup

1. Goto: https://community-scripts.github.io/ProxmoxVE/
1. Run Proxmox VE POST install script
1. Run Proxmox VE LXC Updater scripto
1. Run Proxmox Datacenter Manager


## Dataceneter Manager (alpha)

The old way was to create a cluster all at once. Not add node by node to the cluster. So if you want to add a proxmox node to a cluster you had to reinstall it first. DCM allows you to do that on the fly.

* Migrate things to and from nodes

Allows you to control multiple nodes that are clustererd together.

## Docker Apps

Run them via LXC/ubuntu or LXC/alpine container. LXC uses a bare minimal VM
just enough to run docker so no extra resources get used (better than running
docker via a real VM). LXC = Full VM minus Kernel (gets shared with host)



## New zettel: LXC vs VM vs Docker

VM Virtualization:
* VMs actually virtualize everything from hardware up through the operating system
* They include a complete guest operating system with its own kernel
* Each VM runs independently with its own dedicated resources

Container Virtualization:
* Containers share the host's kernel and don't virtualize it
* They operate above the kernel level but don't virtualize it
* Instead, they provide process isolation and resource management

The key difference lies in the kernel layer:

* VMs have their own complete kernel
* Containers share the host's kernel

LXC Container vs Docker Container:

LXC:
Linux Containers, or LXC, is an advanced virtualization technology that
utilizes key features of the Linux kernel to create lightweight and efficient
isolated environments for running multiple applications on a single host
system.
* Resource management with cgroups
* Isolation with namespaces

Benefits:
* Lightweight nature: Unlike traditional virtual machines that require separate
  operating system (OS) instances, LXC containers share the host system’s
  kernel, making them more resource-efficient and faster to start.
* Proximity to the operating system: Thanks to its integration with the Linux
  kernel, LXC provides functionality similar to that of virtual machines but
  with a fraction of the resource demand.
* Efficient use of system resources: LXC maximizes resource utilization and
  scalability by enabling multiple containers to run on a single host without
  the overhead of multiple OS instances.k

Docker:
Docker offers a comprehensive platform and suite of tools that has
revolutionized how applications are developed, shipped, and run.

Benefits:
* Portability: Containers can be moved effortlessly between environments, from
  development to testing to production, without needing changes, thanks to
  Docker’s ability to ensure consistency across platforms.
* Ease of use: Docker simplifies container management with intuitive commands
  like docker run, significantly lowering the learning curve for new users.
* Vast ecosystem: Docker’s extensive library of container images available on
  Docker Hub and a wide array of management tools support rapid application
  development and deployme

LXC use cases:
* Efficient access to hardware resources: LXC’s close interaction with the host
  OS allows it to achieve near-native performance, which is beneficial for
  applications that require intensive computational power or direct hardware
  access. This can include data-heavy applications in fields like data analysis
  or video processing where performance is critical.
* Virtual Desktop Infrastructure (VDI): LXC is well-suited for VDI setups
  because it can run full operating systems with a smaller footprint than
  traditional VMs. This makes LXC ideal for businesses deploying and managing
  virtual desktops efficiently.

LXC is not typically used for application development but for scenarios
requiring full OS functionality or direct hardware integration. Its ability to
provide isolated and secure environments with minimal overhead makes it
suitable for infrastructure virtualization where traditional VMs might be too
resource-intensive.

Docker use cases:
Docker excels in environments where deployment speed and configuration
simplicity are paramount, making it an ideal choice for modern software
development. Key use cases where Docker demonstrates its strengths include:

* Streamlined deployment: Docker packages applications into containers along
  with all their dependencies, ensuring consistent operation across any
  environment, from development through to production. This eliminates common
  deployment issues and enhances reliability.
* Microservices architecture: Docker supports the development, deployment, and
  scaling of microservices independently, enhancing application agility and
  system resilience. Its integration with Kubernetes further streamlines the
  orchestration of complex containerized applications, managing their
  deployment and scaling efficiently.
* CI/CD pipelines: Docker containers facilitate continuous integration and
  deployment, allowing developers to automate the testing and deployment
  processes. This approach reduces manual intervention and accelerates release
  cycles.
* Extensive image repository and configuration management: Docker Hub offers a
  vast repository of pre-configured Docker images, simplifying application
  setup. Docker’s configuration management capabilities ensure consistent
  container setups, easing maintenance and updates.

Docker’s utility in supporting rapid development cycles and complex
architectures makes it a valuable tool for developers aiming to improve
efficiency and operational consistency in their projects.

## OLD Notes:

# Requirements

* VT-d and VT-x support on motherboard
* two GPUs if you still want to access Proxmox directly, if you only need ssh, and webinterface 1 GPU is enough. Problem is that GPU passthrough will only be available on guest OS the host won't have it. => Luckily I have an APU+GPU (this issue is more concerning on a type 2 hypervisor like VirtualBox if you want GPU passthrough on a win VM you really need 2 GPUs)
* Motherboard+CPU VM support: AMD-VI/VT-d => for hardware passthrough and AMD-V/VT-x to enable VMs
CHECK if your have it enabled/support: https://stackoverflow.com/questions/11116704/check-if-vt-x-is-activated-without-having-to-reboot-in-linux/11118147

If not supported you might need to use FPTW backup and flash bios, use AMIBCP modify the bios(open vt-d).

https://techgenix.com/difference-between-amd-vintel-vt-x-and-amd-viintel-vt-d-188/

Important: Go into your BIOS and enable AMD-VI/IOMMU and AMD-V/SVM

## 1. Install Proxmox
https://github.com/debauchee/barrier

If I had multiple SSD's I would go for a RAID array and probably with ZFS or Btrfs. Since I only have 1 single 250GB SSD I chose ext4 and keep it simple.

1. Important: Go into your BIOS and enable AMD-VI/IOMMU and AMD-V/SVM
2. Simply download the Proxmox ISO and copy it on to a Ventoy formatted USB Stick

## 2. SSH: Update Proxmox Repo and upgrade system

1. either ssh into your proxmox within your local network or directly enter your credentials user=root
2. apt update
3. apt upgrade => error cause proxmox comes with a .gpg key pair for enterprise => subscription only

Fix: Get the test branch of proxmox which is free / if proxmox updates to a new debian => change bullseye keyword to the latest
* `echo "deb http://download.proxmox.com/debian bullseye pve-no-subscription" > /etc/apt/sources.list.d/pve-test.list`
* `curl -LJ http://download.proxmox.com/debian/proxmox-release-bullseye.gpg -o /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg`

3&4. Automate step 3 with script: run dark mode and post install:

https://tteck.github.io/Proxmox/

4. apt update && apt -y full-upgrade
5. reboot

### Why I deleted local-lvm

Currently I only got a 250GB SSD laying around. Hence local-lvm (volume) and local (storage) makes not much sense with so little space to work.
Idea: The local-lvm partition can only store VMs no files/backups or anything else. Where the local partition can store anything. The difference is that the local storage is a folder on the filesystem and the local-lvm is a volume.

```

drive -> partition -> PV -> VG -> LV 'root' -> ext4 mounted on / -> directory /var/lib/vz configured as 'local' storage
                               -> LV 'data' -> LVM thin pool configured as 'local-lvm' storage
```


Local LVM does not need to be removed and should be used to install VM’s and Containers. A VM in an LVM storage will perform faster and allows for features like snapshots without using qcow2, and merging physical volumes for volume groups.
The only reason I see it should be done is if there are extreme storage limits.


## 3. Web Interface: delete local-lvm (only with very limited hdd space, normally I would not recommend)

1. Login to proxmox via IP on your browser
2. Go to "Datacenter" "Storage" and select local-lvm then "remove"
3. Go to "proxmox/yourHostname" "Shell"

```bash
lvremove /dev/pve/data
lvresize -l -r +100%FREE /dev/pve/root
#resize2fs /dev/mapper/pve-root # -r flag already does the same
```

4. allow local partition to store diskimages: "Datacenter" "Storage" select "local" "edit" select "diskimage" => ok


## 4. Web Interface: Enable GPU Passthrough on Win11

\#Try: https://github.com/HikariKnight/quickpassthrough

\#TODO download YT Video and safe it on 2ndHDD + make a script out of it to easily deal with that

https://yewtu.be/watch?v=S6jQx4AJlFw
Idiot Friendly! AMD/NVIDIA GPU Passthrough in Proxmox Guide by Tech Hut
https://gist.github.com/qubidt/64f617e959725e934992b080e677656f Guide


Script to find out IOMMU Group:

```bash
#!/bin/bash
shopt -s nullglob
for d in /sys/kernel/iommu_groups/*/devices/*; do
    n=${d#*/iommu_groups/*}; n=${n%%/*}
    printf 'IOMMU Group %s ' "$n"
    lspci -nns "${d##*/}"
done;
```

Check to see if IOMMU is enabled: `dmesg | grep -e DMAR -e IOMMU`
--------------------- (not sure if needed)
\# Fix Bar 0: cant reserver errror
https://forum.proxmox.com/threads/explaining-snippets-feature.53553/
https://libredd.it/r/VFIO/comments/igc5z0/bar_cant_reserve_and_no_more_images_in_pci_rom/
https://pve.proxmox.com/pve-docs/pve-admin-guide.html#_hookscripts

1. ssh into proxmox: mkdir /var/lib/vz/snippets
2. create script: touch fixGPU

and add:

```bash
#!/bin/bash
for vtconsole in /sys/class/vtconsole/*; do echo 0 | sudo tee $vtconsole/bind ; done # Unbind all vtcon's found temporarily
echo efi-framebuffer.0 | sudo tee /sys/bus/platform/drivers/efi-framebuffer/unbind # Unbind this temporarily
```
---------------------
3. chmod u+x  fixGPU
4. cp fixGPU /var/lib/vz/snippets
5. qm set 100 --hookscript local:snippets/fixGPU
-------------------------

https://github.com/gnif/vendor-reset/issues/46
https://forum.proxmox.com/threads/gpu-passthrough-troubleshooting-constant-bar-0-cant-reserve-mem-errors.82162/
https://libredd.it/r/VFIO/comments/igc5z0/bar_cant_reserve_and_no_more_images_in_pci_rom/
https://forum.proxmox.com/threads/problem-with-gpu-passthrough.55918/
https://forum.proxmox.com/threads/problem-with-gpu-passthrough.55918/#post-361178
https://github.com/gnif/vendor-reset/issues/46#issuecomment-992282166
https://forum.proxmox.com/threads/proxmox-requires-full-reboot-after-shutting-down-vm-with-pci-passthrough.101552/

BIG Question: How to get proxmox with grub to use APU instead of GPU? Then all this error stuff related to VM boot would not happen


Troubleshoot
Check logs after booting proxmox: `journalctl -b 0` also run `dmesg | grep BAR` before and after starting VM
Check if vendor-reset is installed: `dmesg | grep vendor`

echo 'device_specific' > "/sys/bus/pci/devices/0000:12:00.0/reset_method"

1. ssh into proxmox: vi /etc/default/grub
edit line: GRUB_CMDLINE_LINUX_DEFAULT="quiet" change to: GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"
grep/sed
2. echo "vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd" >> /etc/modules
3. echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
4. echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
5. echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf
6. echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
7. echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
8. lspci -v | grep VGA
the first for numbers need to be copied in my case 12:00
9. lspci -n -s 12:00
copy both id's in my case: 1002:73ff 1002:ab28
10. echo "options vfio-pci ids=1002:73ff,1002:ab28 disable_vga=1" > /etc/modprobe.d/vfio.conf
11. update-initramfs -u
12. reboot
13. webinterface: upload windows11 and vsio driver image
14. webinterface: setup Win11 with settings (don't boot it yet!)
15. ssh into proxmox: <vmid> = id of your Windows VM
echo "cpu: host,hidden=1,flags=+pcid
args: -cpu 'host,+kvm_pv_unhalt,+kvm_pv_eoi,hv_vendor_id=NV43FIX,kvm=off'" >> /etc/pve/qemu-server/<vmid>.conf
16. webinterface: add hardware pci device
17 Boot up Win11 and install it
First time boot into win11:
Make sure you have remote access and know your local IP4 address as noVNC might not work once GPU is passthrough
* settings -> enable remote desktop
* device manager -> install only the ethernet driver
* power shell: ipconfig (copy paste the IP4 to somewhere or memorize it)
18. Login with remote desktop client to your windows VM:
install 'virtio guest driver installer' to get all drivers installed
19. webinterface: turn off VM and "Hardware" "Diplay" change from "default" to "none" (not sure if I still need to do that.
20.



## 4. Web Interface: Install Windows VM

1. Download and upload Windows Iso on to the 'local' partition
2. Download and upload Windows [VirtIO] drivers on to the 'locall partition ( I got the latest version since I plan to install win11)
[VirtIO]<https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers>
3. right click 'pve/yourHostname' 'create VM'

\#TODO download YT Video and safe it on 2ndHDD

https://yewtu.be/watch?v=6c-6xBkD2J4
Virtualize Windows 10 with Proxmox VE by techno tim


##
https://github.com/debauchee/barrier


