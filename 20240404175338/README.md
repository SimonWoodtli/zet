# firewalld explained

> 🧐 To check status: `systemctl status firewalld`

firewalld is the Dynamic Firewall Manager. It utilizes network/firewall zones
which have defined levels of trust for network interfaces or connections. It
supports both IPv4 and IPv6 protocols.

In addition, it separates runtime and permanent (persistent) changes to
configuration, and also includes interfaces for services or applications to add
firewall rules.

Config Dirs:

* /etc/firewalld (higher priority)
* /usr/lib/firewalld

>📝 If you have more than one network interface when using IPv4, you have to
>turn on ip forwarding. Persistent: add 'net.ipv4.ip_forward=1' to
>/etc/sysctl.conf then run `sudo sysctl -p`. Or non-persistent: `sudo sysctl net.ipv4.ip_forward=1 && echo
>1 > /proc/sys/net/ipv4/ip_forward`

### Zones

firewalld works with zones, each of which has a defined level of trust and a
certain known behavior for incoming and outgoing packets. Each interface
belongs to a particular zone (normally, it is NetworkManager which informs
firewalld which zone is applicable), but this can be changed with firewall-cmd
or the firewall-config GUI.

> 📝 On system installation, most if not all Linux distributions will select the public zone as default for all interfaces.

Zones:

* drop: All incoming packets are dropped with no reply. Only outgoing connections are permitted.
* block: All incoming network connections are rejected. The only permitted connections are those from within the system.
* public: Do not trust any computers on the network; only certain consciously selected incoming connections are permitted.
* external: Used when masquerading is being used, such as in routers. Trust levels are the same as in public.
* dmz (Demilitarized Zone): Used when access to some (but not all) services are to be allowed to the public. Only particular incoming connections are allowed.
* work: Trust (but not completely) connected nodes to be not harmful. Only certain incoming connections are allowed.
* home: You mostly trust the other network nodes, but still select which incoming connections are allowed.
* internal: Similar to the work zone.
* trusted: All network connections are allowed.


### Commands

> 🧐 Note that you can remove a previously assigned source from a zone by using
>the --remove-source option, or change the zone by using --change-source.

> 🧐 Use `sudo firewall-cmd --reload` to apply changes

Interfaces:

* assign interface ton zone temporarily: sudo firewall-cmd --zone=internal --change-interface=eno1
* assign interface to zone permanent: sudo firewall-cmd --permanent --zone=internal --change-interface=eno1
* list all interfaces within public zone: sudo firewall-cmd --zone=public --list-all
* https://firewalld.org/documentation/man-pages/firewall-cmd.html

IP-Address:

* assign a source to a zone (permanently): sudo firewall-cmd --permanent --zone=trusted --add-source=192.168.1.0/24
* list the sources bound to a zone: sudo firewall-cmd --permanent --zone=trusted --list-sources 192.168.1.0/24

Services:

* list all services available: sudo firewall-cmd --get-services
* list services within a zone: sudo firewall-cmd --list-services --zone=public
* add a service to a zone: sudo firewall-cmd --permanent --zone=home --add-service=dhcp

Ports:

* list ports of a zone: sudo firewall-cmd --zone=home --list-ports
* add port to a zone: sudo firewall-cmd --zone=home --add-port=21/tcp

Port Redirection:

* redirect/forward port 80 to 8080 on external zone: sudo firewall-cmd --zone=external --add-forward-port=port=80:proto=tcp:toport=8080

> 📝 /etc/services contains a list of of all ports and corresponding services

### Source Management

Any zone can be bound not just to a network interface, but also to particular network addresses. A packet is associated with a zone if:

* It comes from a source address already bound to the zone; or if not,
* It comes from an interface bound to the zone.

Any packet not fitting the above criteria is assigned to the default zone (i.e, usually public).

### Port Redirection

> 🧐 When using Port redirection be as specific as possible.

During packet ingress, sometimes it is desirable to change the destination
port. When remapping ports, it is best to specify the zone and optionally an IP
address to make the rule as specific as possible. This will avoid generalized
rules capturing unintended packets.

Tags:

    #linux #sysadmin #networking #traffic #firewall #LFS207
