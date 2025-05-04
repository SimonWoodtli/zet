# Proxmox Install Guide

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

## 2. Setup: Scripts

1. Goto: https://community-scripts.github.io/ProxmoxVE/
1. Run Proxmox VE POST install script
  * corrects repos sources (removes enterprise adds non subscription)
  * adds ceph repo
  * adds pvetest repo
  * disable subscription reminder/notifier on website
  * enables HA for clustered env. (high availability: https://pve.proxmox.com/wiki/High_Availability)
    * enable Local Resource Manager service
    * enable Cluster Resource Manager
    * enable Corosync clustering engine
  * disables HA for single node env.
  * apt update system and reboots
1. Run Proxmox VE LXC Updater script
  * Runs an apt-update or whatever your container is to update all installed pkgs in container (Does not upgrade a single app version when new one is released)
1. Run Proxmox Datacenter Manager

TODO: check/read all those bash scripts to understand what they are changing.

### 2.1 Setup: Storage

Bootdrive dependent: keep or remove local-lvm partition

> ⚠️ If you decide to use ZFS make sure to have enough RAM on your system 32GB min.
> For speed it is recommended to have your bootdrive as NVMe m.2 like with any other System.
> IF you use ZFS it requires RAM (ARC/cache). About 1GB of RAM for every 1TB of storage.

Decision:

* If you have a big bootdrive 1TB+ you can keep the local-lvm volume.
* If you have a small bootdrive removing local-lvm and allocate the storage to
  local makes sense.
* If you have multiple nvmes (2 or 4) you probably also want to keep local-lvm. You'd
  want to create a ZFS pool during the proxmox installer so your bootdrive is
  also saved from disk disk failure and your local-lvm too.
* If you have 3 nvmes and one is small it might make sense to go bootdrive with
  small without local-lvm and the other 2 nvme as ZFS Mirror pool.

Proxmox default partitions:

The local-lvm partition can only store VMs no files/backups or anything else.
Where the local partition can store anything. The difference is that the local
storage is a folder on the filesystem and the local-lvm is a volume

* local (here you add boot isos, images/templates)
* local-lvm (here you add your running vms and containers), you do need to back them up in case of disk failure.

Storage:

If you have a small bootdrive you probably want to create two ZFS pools. A storage pool and a Container/VM pool.
* One for storage, 3.5hdds: depends on how many if 2 go Mirror if more RaidZ or whatever makes sense
* One for containers/vms, nvmes: Use two nvmes and raid lvl Mirror
  * (If your MOBO has more nvme slots just create a second storage pool or whatever you like)
* (Second fast storage, 2.5ssd): depends on how many if 2 go Mirror if more RaidZ or whatever makes sense

If you have a big bootdrive you can get away with only a storage ZFS pool.
However you do need to backup your VMs/containers in local-lvm in that case.
This is why I think it is cleaner to go with a 500GB bootdrive and only have a
local partition on your bootdrive.

https://www.45drives.com/community/articles/RAID-and-RAIDZ/

### 2.2 Setup: Add ZFS pools

This depends widely on how many drives, which drives etc. as a general idea for
a VM Server it be nice to have at least a nvme and hdd pool.

If you have 4 nvme it makes sense to have 2nmve as a ZFS pool created when
running proxmox. And 2 nvmes as ZFS RAID1 when you install proxmox in the
Installer. That way you can keep the local-lvm partition for containers/vms
because disk failure is not a problem. And you could use the other ZFS pool for
either more container/vms or fast data. (
RESEARCH: maybe 4 at installer would make more sense?

If you have 1 nvme just use it as your boot and use local-lvm for
containers/vms but backing up your containers/vms is IMPORTANT!

If you have 2 or 3 nvmes I'd setup a ZFS pool during the proxmox installer and
use local-lvm.

1. nvme pool: (if you have two = mirror, if 3 or more raidz) for running VMs, Containers
2. hdd pool: (if you have two = mirror, if 3 or more raidz) for data storage for your NAS or VMs
3. ssd pool: (if you have two = mirror, if 3 or more raidz) for fast data storage for your NAS or VMs or as a cache

> If they already have been used and have data on it you need to 'wipe disks' first.

On the webgui select the node then 'Disks' then 'ZFS' and select your drives. Make sure to have 'Add storage' selected and use the correct RAID level.

1. Get total storage: `zpool list`
1. Get actual available storage: `zfs list`

On the webgui 'Datacenter' 'Storage' check the content and make sure your pools are setup for the right content: disk image, containers and your 'local' for Backup, ISO image, Container template

### 2.3 Setup: remove local-lvm

Only do this if you are sure you don't want a local-lvm in most cases you probably want to keep a local-lvm.

```
pvesm status
lvremove /dev/pve/data
lvresize -l -r +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root
pvesm status
```

Research/test: can local new proxmox installs store container images, isos, vms images directly or is further editing of local required?
4. allow local partition to store diskimages: "Datacenter" "Storage" select "local" "edit" select "diskimage" => ok

### 2.4 Setup: Enable IOMMU for I/O and GPU passthrough

> If you passthrough a GPU to a VM the host won't have access to it. So it can only ever be available at one machine at a time.

IMPORTANT: Motherboard+CPU VM support: AMD-VI/AMD-V or Intel VT-d/VT-x

1.

BIOS: AMD
* Enable AMD-VI/IOMMU and AMD-V/SVM

BIOS: Intel
* Enable VT-d and VT-x

2. Check to see if IOMMU is enabled: `dmesg | grep -e DMAR -e IOMMU`

3. Search IOMMU grp:

```bash
#!/bin/bash
shopt -s nullglob
for d in /sys/kernel/iommu_groups/*/devices/*; do
    n=${d#*/iommu_groups/*}; n=${n%%/*}
    printf 'IOMMU Group %s ' "$n"
    lspci -nns "${d##*/}"
done;
```


## 3 Setup: Create your first LXC Container

> Match the Container ID to the end number of your local IP. E.g. 192.168.1.120 would be Container ID 120

## 4 Setup: Create your first VM


## New Zettel Proxmox general Info:

Things you can install or have on your node:

* LXCs
* Container images/templates: You can safe your LXC container into an image then mess
  around with your LXC container and if things break. You can delete the LXC
  container and create a new LXC from the image. (Same concept to how docker
  container/images work)
* VMs

## Datacenter Manager (alpha)

The old way was to create a cluster all at once. Not add node by node to the cluster. So if you want to add a proxmox node to a cluster you had to reinstall it first. DCM allows you to do that on the fly.

* Migrate things to and from nodes

Allows you to control multiple nodes that are clustererd together.

## Docker Apps

Run them via LXC/ubuntu or LXC/alpine container. LXC uses a bare minimal VM
just enough to run docker so no extra resources get used (better than running
docker via a real VM). LXC = Full VM minus Kernel (gets shared with host)

## New zettel: LXC vs VM vs Docker

> ⚠️ If you create a VM and have a proxmox cluster you can do live migration
> (whilst VM keeps running the VM gets copied onto new Server and never shuts
> down, with Containers you cannot do live migration they need to be shut down)

> ⚠️ Reallocating CPU/Memory/Storage resources works on the fly with Containers. However on VMs you need to stop the machine before changing any resources.

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

Related:

* [20250504133640](/20250504133640/) ZFS: Raids explained
