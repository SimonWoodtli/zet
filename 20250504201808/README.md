# Install and Setup TrueNAS via Proxmox VM

1. Enter Proxmox webgui and go to your node where you want to install TrueNAS on. Then select the 'local' storage and ISO images. From there with 'Download from URL' you can enter the TrueNAS download link.
2. Click on 'Create VM'
3. Go through all the settings, on 'System' and 'Network' you can leave everthing on default.
  * Disks: add 32GB of nvme storage to install the OS
  * For the actual storage hdds it is not recommended to add a proxmox managed pool but instead do a direct PCIE passthrough of the hdds.
  * CPU: min. 4 cores, type: host (meaning its not emulating a different cpu but uses what your actual hardware cpu is)
  * Memory: min 8192GB

## Storage: Passthrough your disks

Now there are two ways you might end up having storage available. Direclty on your motherboard via M.2 Slots, SATA ports or via a PCIE controller card that expands your sata/sas hdds.

If you have a PCIE controller card:

1. Select TrueNAS VM
1. Go to 'Hardware' and 'Add' 'PCIE device' and select the card which should then add all the connected hdds to it.


If you want to directly passthrough from your motherboard:

1. SSH into your proxmox
1. Find out your devices id: `lsblk -o +MODEL,SERIAL` then look for the one that matches within /etc/disk/by-id: `ls /dev/disk/by-id`
1. On proxmox webGUI you have a naming scheme like 'number (hostname)' for VMs/Containers the number is the <VM-ID>
1. Add the drive to your VM e.g. as scsi1: `qm set <VM-ID> <-scsi1> /dev/disk/by-id/<your unique disk ID>`
1. Repeat the process for all the disks you want to add. scsi2 scsi3 etc.

## Setup

1. Start the VM and go through the installer
1. Go to the webGUI <yourIP:??>

Related:

* [20220903203700](/20220903203700/) Proxmox Install Guide
* [20250504133640](/20250504133640/) ZFS: Raids explained
