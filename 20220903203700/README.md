# Proxmox Notes => needs cleanup

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


