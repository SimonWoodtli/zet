# /etc/hosts file explained

The /etc/hosts file holds a local database of hostnames and IP addresses. It
contains a set of records (each taking one line) which map IP addresses with
corresponding hostnames and aliases.

A typical file looks like:

```
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.1.100 hans hans7 hans64
192.168.1.150 bethe bethe7 bethe64
192.168.1.2 hp-printer
192.168.1.10 test32 test64 oldpc
```

Such static name resolution is primarily used for local, small, isolated
networks. It is generally checked before DNS is attempted to resolve an
address.

## Other hosts related files

* The other host-related files in /etc are /etc/hosts.deny and
  /etc/hosts.allow. These are self-documenting and their purpose is obvious
  from their names. The allow file is searched first and the deny file is only
  searched if the query is not found there.
* /etc/host.conf contains general configuration information; it is rarely used.

Tags:

    #linux #sysadmin #networking #wlan #ethernet #interfaces #devices
