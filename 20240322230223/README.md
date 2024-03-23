# Firewalls explained

A firewall is a network security device that monitors and controls incoming and
outgoing network traffic based on a set of predefined security rules. It acts
as a barrier between a trusted internal network and untrusted external
networks, such as the Internet, to prevent unauthorized access and protect the
network from threats.

## Programs

> 🧐  firewalld is easy to configure and replaces the older iptables. A modern low level firewall replacement for iptables would be nftables.

> ⚠️ Don't run two firewall services on a single machine. Pick your firewall and stick with that.

All these firewalls run as services:

* `ufw`
* firewalld: `firewall-cmd`
* `iptables` (
* `nftables`

---------

## firewalld

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



--------------

## Network Address Translation (NAT)

Network Address Translation (NAT) is a method the firewall uses to change the
packet source or destination address as it enters or exits the firewall. There
are several types of NAT:

* DNAT (Destination Network Address Translation)
* SNAT (Source Network Address Translation)
* Masquerade, a variation of SNAT


### DNAT

DNAT (Destination Network Address Translation) alters the end destination (the
“to” IP address) and possibly the destination port, as the packet enters the
system from the Internet.

* When a packet is sent, it will have the public IP address provided by DNS
* The public IP address will be that of the firewall
* Changing the destination of the packet is necessary to allow the packet to
  move through the firewall to the internal network to the desired service

The DNAT firewall rule is normally placed in the pre-routing chain of the NAT
table. This would be the Internet-facing adapter and the public address used by
services like a web server.

Since firewalls are typically pass-through devices (few local services), the
packet is examined and the destination port examined to see if there is an
applicable rule. Our rule will examine the packet for:

* Destination IP address; it should be that of the firewall
* Destination port number; it will be the port number of the service (http=80)
* The DNAT rule will change the destination IP address to the address of the web server behind the firewall
* The packet will be forwarded to the web server behind the firewall
* The web server accepts and processes the packet and sends its reply

### SNAT and Masquerade

Source Network Address Translation (SNAT) changes the source (the "from") IP
address on the packet as it leaves the system on the way to the Internet.

* Translation is required to protect the addresses used on internal networks
  from exposure to the Internet
* Most internal networks are private networks and not routable

If the IP address on the outbound interface is provided by DHCP, the Masquerade
option is used to implement SNAT. The masquerade option requires more compute
resources than SNAT.

If the outbound interface is static, SNAT can be used and the public IP address is specified.

SNAT and masquerade are in the post-routing chain of the NAT table.

## History

Early firewalls (dating back to the late 1980s) were based on packet filtering:
the content of each network packet was inspected and was either dropped,
rejected, or sent on. No consideration was given about the connection state:
what stream of traffic the packet was part of.

The next generation of firewalls were based on stateful filters, which also
examine the connection state of the packet to see if it is a new connection,
part of an already existing one, or part of none. Denial of service
attacks can bombard this kind of firewall to try and overwhelm it.

The third generation of firewalls is called Application Layer Firewalls, and
are aware of the kind of application and protocol the connection is using. They
can block anything which should not be part of the normal flow.​

Related:

* <https://www.linkedin.com/pulse/most-popular-linux-firewalls-mikhail-matyunin>

Tags:

    #linux #sysadmin #networking #traffic #firewall #LFS207
