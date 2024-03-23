# Firewalls explained

A firewall is a network security device that monitors and controls incoming and
outgoing network traffic based on a set of predefined security rules. It acts
as a barrier between a trusted internal network and untrusted external
networks, such as the Internet, to prevent unauthorized access and protect the
network from threats.

## Programs

> 🧐  As a service, firewalld replaces the older iptables. It is an error to
> run both services, firewalld and iptables, at the same time.

* `ufw`
* firewalld: `firewall-cmd`
* `iptables`
* `nftables`

## firewalld

firewalld is the Dynamic Firewall Manager. It utilizes network/firewall zones
which have defined levels of trust for network interfaces or connections. It
supports both IPv4 and IPv6 protocols.

In addition, it separates runtime and permanent (persistent) changes to
configuration, and also includes interfaces for services or applications to add
firewall rules.

Config Dirs:

* /etc/firewalld (higher priority)
* /usr/lib/firewalld

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
