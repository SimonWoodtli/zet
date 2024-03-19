# Networking create a default route

## Non-persistent

```
sudo ip route add default via 192.168.1.10 dev enp2s0
ip route
```

## Distro independent

```
sudo nmcli con mod virbr0 ipv4.routes 192.168.10.0/24 +ipv4.gateway 192.168.122.0
sudo nmcli con up virbr0
```

## Red Hat

Modify the /etc/sysconfig/network file.

```
GATEWAY=x.x.x.x
```

Or in /etc/sysconfig/network-scripts/ifcfg-ethX on a device-specific basis.

## Debian

Modify the /etc/network/interfaces file.

```
gateway=x.x.x.x
```

Tags:

    #linux #sysadmin #networking #wlan #ethernet #interfaces #devices
