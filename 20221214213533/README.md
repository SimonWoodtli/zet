# Networking: Useful Commands to Troubleshoot and Manage network interfaces and DNS on Linux

> 🧐 Fedora and Ubuntu use `resolvectl` and the `systemctl` service: systemd-resolved.service. They also use the NetworkManager

> 🧐 Network Manager stores profile settings here: /etc/NetworkManager/system-connections/*

/etc/resolve.conf get set automatically. I couldn't figure out how to change settings there with Fedora

interfaces = network adapters

* `nmcli` is the CLI version for NetworkManager
* current network overview: `nmcli`
* more detailed network overview: `nmcli device show`
* show all known network profiles that you have connected to: `nmcli connection show`
* change DNS permanent on an interface: `sudo resolvectl dns <interface>`
* change DNS temporarily: `sudo ip r a default via <DNS-IP>`
* remove DNS settings from an interface: `sudo resolvectl revert <interface>`
* to check the status of DNS: `resolvectl status` and `systemctl status systemd-resolved.service`
* ip address info for all interfaces: `ip a`
* get only local ip: `hostname -I`
* routing info: `ip r`
* link info: `ip l`

Networking on Linux is fine until it's not :)

Related:

* <https://www.computernetworkingnotes.com/linux-tutorials/network-configuration-files-in-linux-explained.html>

Tags:
    
    #linux #network #dns #ip #troubleshoot
