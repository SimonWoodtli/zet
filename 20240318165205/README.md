# How does the NTP protocol work?

> ⚠️ You cannot have systemd-timedated and ntp installed at the same time. If
> you want to install ntp remove timedated: `sudo apt remove systemd-timesyncd`
> first.

> 🧐 On Ubuntu and Fedora it ships by default with systemd timedated and if you
> install crony time gets synced via that service (and timedated gets disabled). Both these implementations
> are simpler implementations of ntp and easier to configure/use in most cases
> they are sufficient. However if you do need more control ntpd (ntp pkg) the
> old way may still be useful.

The configuration elements for the ntp server are contained in the /etc/ntp.conf file. Here are some of the ntp server relevant items:

* Declare the local machine to be a time reference
    * The server and fudge directives
* Regulate who can query the time server with ntpq and ntpdc commands (see CVE-2013-5211)
    * The restrict directives
* Declare which systems are ntp peers
    * The peers are listed in the /etc/ntp.conf file
* Start NTP daemon

## Config file: Using pool vs server

A pool is a group of servers managed by a single instance that you can
"subscribe" to sync. A server is a single server you can "subscribe" to sync.

In other words the main difference between server and pool directives in the
ntp.conf file is that server is used for specifying a single NTP server, while
pool is used for specifying a group of NTP servers managed by a pool. Using a
pool can provide better load distribution and availability compared to using a
single server.

Example in /etc/ntp.conf:

```
pool xxx.xxx.xxx.xxx
server xxx.xxx.xxx.xxx
```

## Config file: Using peer

The peers directive in the ntp.conf file is used to establish a peer
relationship between two NTP servers. Unlike the server directive, which
specifies a server to synchronize with, the peers directive is used to
configure two NTP servers to synchronize with each other. This setup is
particularly useful in scenarios where you have multiple NTP servers and you
want them to synchronize their time with each other, ensuring that if one
server fails or loses its external time source, the other can still provide
time to the clients on the network.

In a peer relationship, both NTP servers act as both clients and servers to
each other. This means that if one server loses its external time source, it
can still provide time to the other server, which in turn can provide time to
the clients. This redundancy helps in maintaining time synchronization across
the network even in the event of a failure of one of the servers or their
external time sources

Example in /etc/ntp.conf:

```
peer 192.168.1.1
```

## Create Access Control

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

Related:

* https://ubuntu.com/server/docs/use-timedatectl-and-timesyncd
* https://www.digitalocean.com/community/tutorials/how-to-set-up-time-synchronization-on-ubuntu-20-04
* https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/s1-understanding_the_ntpd_configuration_file
* https://sookocheff.com/post/time/how-does-ntp-work/
* [20240318165634](/20240318165634/) Configuring the ntpd Client

Tags:

    #linux #sysadmin #time #NTP #timedatectl #ntpq #ntpdc
