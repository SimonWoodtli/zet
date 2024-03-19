# Network Devices/Interfaces: Common config file locations

> ⚠️  When using systemd (systemd is getting more standardized), it is often
> preferable to use Network Manager.

On newer Linux distributions, these configuration files may be gone or empty.

## Red Hat

```
/etc/sysconfig/network
/etc/sysconfig/network-scripts/ifcfg-ethX
/etc/sysconfig/network-scripts/ifcfg-ethX:Y
/etc/sysconfig/network-scripts/route-ethX
```

## Debian

```
/etc/network/interfaces
```

## Suse

```
/etc/sysconfig/network
```

Related:

* [20221214213533](/20221214213533/) Networking: Useful Commands to Troubleshoot and Manage network interfaces and DNS on Linux
* [20240319181218](/20240319181218/) Network Devices/Interfaces: Common config file locations

Tags:

    #linux #sysadmin #networking #wlan #ethernet #interfaces #devices
