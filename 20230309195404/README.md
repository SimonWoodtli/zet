# Silverblue install KVM/Qemu with virt-manager

## Install

1. On regular Fedora installing virt-manager: `sudo dnf install @virtualization` or `sudo dnf group install --with-optional virtualization` 
    1. start the service `sudo systemctl enable --now libvirtd` 
    1. add user to libvirt group `sudo usermod -aG libvirt <username>`

However Silverblue has no dnf command, and the rpm-ostree command does not support group commands. If you run a toolbox which is a Fedora container you can run `dnf group info virtualization` to see a list of all the mandatory packages for virt-manager to be installed.

1. On Fedora Silverblue: `rpm-ostree install virt-install virt-manager virt-viewer`

Tools that help:
* already ships with silverblue: `dnsmasq, netcat 'nc', qemu`
* vde2
* bridge-utils
* libguestfs: tool to manipulate iso images

## Virt-Managers Settings

1. Edit -> Preferences => enable XML editing
1. Edit -> Connection Details -> Virtual Networking => make sure its active and NAT enabled

## VMs

I think I will try spice and check if it also offers drivers like virtio and see how it goes next time I setup a win VM.

> 🧐 Just like virtio tools, virt-manager has its own guest tools called spice. I don't know if spice is worth the hassle but it's there for your Windows VMs. So you need the `spice-guest-tools` and if you want folder sharing with the host `Spice WebDAV daemon`. To launch with WebDAV file sharing use: `virt-viewer --connect=qemu:///system --domain-name win10`

Related:

* <https://virt-manager.org/>
* <https://www.spice-space.org/download.html>
* <https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/?C=M;O=D>
* <https://discussion.fedoraproject.org/t/virtual-machine-manager-bridged-network-why-so-complicated-to-achiev/38979>
* <https://www.redhat.com/sysadmin/setup-network-bridge-VM>
* <https://wiki.archlinux.org/title/Libvirt>
* <https://www.ibm.com/docs/en/linux-on-systems?topic=choices-kvm-default-nat-based-networking>
