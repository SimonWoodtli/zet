# Configuring the ntpd Server

The configuration elements for the ntp server are contained in the /etc/ntp.conf file. Here are some of the ntp server relevant items:

* Declare the local machine to be a time reference
    * The server and fudge directives
* Regulate who can query the time server with ntpq and ntpdc commands (see CVE-2013-5211)
    * The restrict directives
* Declare which systems are ntp peers
    * The peers are listed in the /etc/ntp.conf file
* Start NTP daemon

## Access Control

Here is an example of access control entries in /etc/ntp.conf.

/etc/ntp.conf: access control

```
# Default policy prevents queries
restrict default nopeer nomodify notrap noquery
# Allow queries from a particular subnet
restrict 123.123.x.0 mask 255.255.255.0 nopeer nomodify notrap
# Allow queries from a particular host
restrict 131.243.1.42 nopeer nomodify notrap noquery
# Unrestrict localhost
restrict 127.0.0.1
```

## Peer Configuration

Here is an example of ntp peer configuration entries.

/etc/ntp.conf: peer configuration

```
peer 128.100.49.12
peer 192.168.0.1
```

## Declaring Self a Time Source

This is an example of the ntp declaring itself as an NTP server. The second line, the fudge entry, specifies this server is a stratum 10 server.

/etc/ntp.conf: declare self a time source

```
server 127.127.1.0
fudge 127.127.1.0 stratum 10
```

Tags:

    #linux #sysadmin #time #NTP #timedatectl #ntpq #ntpdc
