# Networking create a static route

Either the `route` or `ip` command can be used to set a non-persistent route as in: (after restart these rules go away)

* example: `sudo ip route add 10.5.0.0/16 via 192.168.1.100`

## Red Hat

A persistent route can be set by editing the /etc/sysconfig/network-scripts/route-ethX file as shown by the following command and its output:

```
10.5.0.0/16 via 172.17.9.1
```

## Debian

A persistent route can be set by adding lines to the /etc/network/interfaces file, such as:

```
iface eth1 inet dhcp
post-up ip route add 10.1.2.51 dev eth1
post-up ip route add 10.1.2.52 dev eth1
```

## Suse

On a SUSE-based system you need to add to or create a file such as /etc/sysconfig/network/ifroute-eth0 with lines like:

```
# Destination Gateway Netmask Interface [Type] [Options]
192.168.1.150 192.168.1.1 255.255.255.255 eth0
10.1.1.150 192.168.233.1.1 eth0
10.1.1.0/24 192.168.1.1 - eth0
````

Tags:

    #linux #sysadmin #networking #wlan #ethernet #interfaces #devices
