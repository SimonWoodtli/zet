# Linux Network management

```sh
#!/bin/sh

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## NETWORKING
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# ping (8) - send ICMP ECHO_REQUEST to network hosts
# hostname (1) - show or set the system's host name
# arp (8) - manipulate the system ARP cache
# inetutils-traceroute (1) - Trace the route to a host
# netstat (8) - Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships
# tcpdump (8) - dump traffic on a network

# send ICMP ECHO_REQUEST to 'example.com' -- interrupt with Ctrl-c to quit
ping example.com
# PING example.com (93.184.216.34) 56(84) bytes of data.
# 64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=1 ttl=52 time=201 ms
# 64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=2 ttl=52 time=225 ms
# 64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=3 ttl=52 time=244 ms
# ^C
# --- example.com ping statistics ---
# 3 packets transmitted, 3 received, 0% packet loss, time 2004ms
# rtt min/avg/max/mdev = 201.991/223.954/244.408/17.353 ms

# send ICMP ECHO_REQUEST to 'example.com' three times
ping -c 3 example.com
# PING example.com (93.184.216.34) 56(84) bytes of data.
# 64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=1 ttl=52 time=165 ms
# 64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=2 ttl=52 time=187 ms
# 64 bytes from 93.184.216.34 (93.184.216.34): icmp_seq=3 ttl=52 time=204 ms
#
# --- example.com ping statistics ---
# 3 packets transmitted, 3 received, 0% packet loss, time 2009ms
# rtt min/avg/max/mdev = 165.847/186.036/204.693/15.895 ms

# display system hostname
hostname
# gokcehan-VirtualBox

# display ip address
hostname -I
# 10.0.x.x

# check external ip address using a third-party service
curl ifconfig.me
# 193.140.xxx.xxx

# display current arp table
arp
# Address                  HWtype  HWaddress           Flags Mask            Iface
# ardic.cc.boun.edu.tr             (incomplete)                              eno1
# xxx.xxx.xxx.1            ether   xx:xx:xx:xx:xx:xx   C                     eno1
# tesla.me.boun.edu.tr     ether   xx:xx:xx:xx:xx:xx   C                     eno1
# simurg.cc.boun.edu.tr            (incomplete)                              eno1

# display current arp table with numerical addresses
arp -n
# Address                  HWtype  HWaddress           Flags Mask            Iface
# xxx.xxx.xxx.20                   (incomplete)                              eno1
# xxx.xxx.xxx.1            ether   xx:xx:xx:xx:xx:xx   C                     eno1
# xxx.xxx.xxx.69           ether   xx:xx:xx:xx:xx:xx   C                     eno1
# xxx.xxx.xxx.50                   (incomplete)                              eno1

# track route packets to 'example.com'
traceroute example.com
# traceroute to example.com (93.184.216.34), 30 hops max, 60 byte packets
#  1  193.140.xxx.1 (193.140.xxx.1)  4.925 ms  5.146 ms  5.806 ms
#  2  192.168.200.1 (192.168.200.1)  2.868 ms  3.289 ms  3.378 ms
#  3  192.168.192.70 (192.168.192.70)  0.760 ms  0.774 ms  0.932 ms
#  4  193.140.194.1 (193.140.194.1)  1.435 ms  1.449 ms  1.684 ms
#  5  193.255.0.217 (193.255.0.217)  3.011 ms  3.018 ms  3.221 ms
#  6  193.140.0.149 (193.140.0.149)  9.441 ms  8.718 ms  9.585 ms
#  7  host-85-29-25-9.reverse.superonline.net (85.29.25.9)  8.929 ms  9.135 ms  9.180 ms
#  8  * * *
#  9  * * *
# 10  * * *
# 11  * * *
# 12  * * *
# 13  * * *
# 14  ix-ae-10-0.tcore1.IT5-Istanbul.as6453.net (5.23.0.37)  13.802 ms  13.814 ms  13.787 ms
# 15  if-ae-8-2.tcore1.FNM-Frankfurt.as6453.net (195.219.156.21)  63.382 ms  63.929 ms  62.853 ms
# 16  if-ae-12-2.tcore2.FNM-Frankfurt.as6453.net (195.219.87.1)  51.188 ms  54.153 ms  61.829 ms
# 17  ffm-b1-link.telia.net (213.248.82.40)  55.953 ms  57.335 ms  55.975 ms
# 18  ffm-bb3-link.telia.net (62.115.137.128)  56.951 ms  53.125 ms ffm-bb4-link.telia.net (62.115.137.166)  54.949 ms
# 19  ash-bb4-link.telia.net (62.115.141.108)  150.267 ms hbg-bb4-link.telia.net (213.155.135.141)  69.194 ms ash-bb4-link.telia.net (62.115.141.108)  157.538 ms
# 20  ash-b1-link.telia.net (213.155.136.39)  159.854 ms kbn-bb3-link.telia.net (62.115.115.96)  74.760 ms ash-b1-link.telia.net (213.155.136.39)  146.984 ms
# 21  edgecast-ic-315151-ash-b1.c.telia.net (195.12.254.154)  150.414 ms edgecast-ic-315152-ash-b1.c.telia.net (213.155.141.226)  149.454 ms edgecast-ic-315151-ash-b1.c.telia.net (195.12.254.154)  147.807 ms
# 22  nyk-bb3-link.telia.net (80.91.247.127)  138.630 ms 152.195.65.133 (152.195.65.133)  154.188 ms 152.195.64.133 (152.195.64.133)  156.731 ms
# 23  93.184.216.34 (93.184.216.34)  151.871 ms  152.085 ms ash-b1-link.telia.net (80.91.248.157)  150.086 ms

# display network interface table
netstat -i
# Kernel Interface table
# Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
# eno1       1500 0    590598      0      0 0        235380      0      0      0 BMRU
# lo        65536 0       534      0      0 0           534      0      0      0 LRU

# display kernel routing table
netstat -r
# Kernel IP routing table
# Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
# default         xxx.xxx.xxx.1   0.0.0.0         UG        0 0          0 eno1
# link-local      *               255.255.0.0     U         0 0          0 eno1
# xxx.xxx.xxx.2   xxx.xxx.xxx.1   255.255.255.255 UGH       0 0          0 eno1
# xxx.xxx.xxx.0   *               255.255.255.0   U         0 0          0 eno1

# display kernel routing table with numerical addresses
netstat -rn
# Kernel IP routing table
# Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
# 0.0.0.0         xxx.xxx.xxx.1   0.0.0.0         UG        0 0          0 eno1
# xxx.xxx.xxx.0   0.0.0.0         255.255.0.0     U         0 0          0 eno1
# xxx.xxx.xxx.2   xxx.xxx.xxx.1   255.255.255.255 UGH       0 0          0 eno1
# xxx.xxx.xxx.0   0.0.0.0         255.255.255.0   U         0 0          0 eno1

# display statistics for each protocol
netstat -s
# (displays the information)

# display all open ports
netstat
# (displays the information)

# display all listening ports
netstat -l
# (displays the information)

# display all open ports with program names
netstat -p
# (displays the information)

# capture all network packets
sudo tcpdump
# (shows packets)

# capture all network packets without converting addresses
sudo tcpdump -n
# (shows packets)

# capture all network packets using src or dst port 22
sudo tcpdump port 22
# (shows packets)

# capture all network packets using port 20 or 21
sudo tcpdump port 20 or port 21
# (shows packets)

# capture all network packets using port 80 and print in ascii form
sudo tcpdump port 80 -A
# (shows packets)

# capture all network packets and save to 'foo.pcap' file
sudo tcpdump -w foo.pcap
# (writes packets to the file)

# select all network packets in 'foo.pcap' file using port 80 and print in ascii form
tcpdump -r foo.pcap port 80 -A
# (reads packets from the file)

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## CONFIGURATION
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# ifconfig (8) - configure a network interface
# ifup (8) - bring a network interface up
# ifdown (8) - take a network interface down
# ip (8) - show / manipulate routing, devices, policy routing and tunnels

# display statuses of currently active interfaces
ifconfig
# eno1      Link encap:Ethernet  HWaddr xx:xx:xx:xx:xx:xx
#           inet addr:xxx.xxx.xxx.xxx  Bcast:xxx.xxx.xxx.255  Mask:255.255.255.0
#           inet6 addr: fe80::b3b3:46b1:7592:cb6/64 Scope:Link
#           UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
#           RX packets:604008 errors:0 dropped:0 overruns:0 frame:0
#           TX packets:238267 errors:0 dropped:0 overruns:0 carrier:0
#           collisions:0 txqueuelen:1000
#           RX bytes:608197341 (608.1 MB)  TX bytes:31493090 (31.4 MB)
#           Interrupt:20 Memory:ec100000-ec120000
#
# lo        Link encap:Local Loopback
#           inet addr:127.0.0.1  Mask:255.0.0.0
#           inet6 addr: ::1/128 Scope:Host
#           UP LOOPBACK RUNNING  MTU:65536  Metric:1
#           RX packets:534 errors:0 dropped:0 overruns:0 frame:0
#           TX packets:534 errors:0 dropped:0 overruns:0 carrier:0
#           collisions:0 txqueuelen:1000
#           RX bytes:42160 (42.1 KB)  TX bytes:42160 (42.1 KB)
#

# disables 'eno1' interface
sudo ifconfig eno1 down
# (interface is disabled)

# set MAC address of 'eno1' interface to '00:00:00:00:00:01'
sudo ifconfig eno1 hw ether 00:00:00:00:00:01
# (MAC address is changed)

# change MTU size of 'eno1' interface to '1492'
sudo ifconfig eno1 mtu 1492
# (MTU size is changed)

# enables 'eno1' interface
sudo ifconfig eno1 up
# (interface is enabled)

# disables 'eno1' interface
sudo ifdown eno1
# (interface is disabled)

# enables eno1 interface
sudo ifup eno1
# (interface is enabled)

# display addresses of all network interfaces
ip addr
# 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
#     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
#     inet 127.0.0.1/8 scope host lo
#        valid_lft forever preferred_lft forever
#     inet6 ::1/128 scope host
#        valid_lft forever preferred_lft forever
# 2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
#     link/ether 00:00:00:00:00:01 brd ff:ff:ff:ff:ff:ff
#     inet xxx.xxx.xxx.49/24 brd 193.140.xxx.255 scope global dynamic eno1
#        valid_lft 1197sec preferred_lft 1197sec
#     inet6 fe80::b3b3:46b1:7592:cb6/64 scope link
#        valid_lft forever preferred_lft forever

# display states of all network interfaces
ip link
# 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
#     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
# 2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
#     link/ether 00:00:00:00:00:01 brd ff:ff:ff:ff:ff:ff

# display kernel routing table
ip route
# default via xxx.xxx.xxx.1 dev eno1  proto static  metric 100
# xxx.xxx.xxx.0/16 dev eno1  scope link  metric 1000
# xxx.xxx.xxx.2 via xxx.xxx.xxx.1 dev eno1  proto dhcp  metric 100
# xxx.xxx.xxx.0/24 dev eno1  proto kernel  scope link  src xxx.xxx.xxx.124  metric 100

# disable 'eno1' interface
sudo ip link set eno1 down
# (interface is disabled)

# enable 'eno1' interface
sudo ip link set eno1 up
# (interface is enabled)

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## NAME RESOLUTION
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# nslookup (1) - query Internet name servers interactively
# host (1) - DNS lookup utility
# dig (1) - DNS lookup utility

# Domain Name System (DNS)
# ~~~
# Translates domain names to IP addresses
# Known hosts are listed in `/etc/hosts`
# DNS servers are configured in `/etc/resolv.conf`

# query the address of a site
nslookup example.com
# Server:         193.140.192.20
# Address:        193.140.192.20#53
#
# Non-authoritative answer:
# Name:   example.com
# Address: 93.184.216.34

# query the address of a site using a different DNS server
nslookup example.com 8.8.8.8
# Server:		8.8.8.8
# Address:	8.8.8.8#53
#
# Non-authoritative answer:
# Name:	example.com
# Address: 93.184.216.34
#

# reverse query an IP address
nslookup 93.184.216.34
# Server:         193.140.192.20
# Address:        193.140.192.20#53
#
# ** server can't find 34.216.184.93.in-addr.arpa.: NXDOMAIN

# query the address of a site
host example.com
# example.com has address 93.184.216.34
# example.com has IPv6 address 2606:2800:220:1:248:1893:25c8:1946

# query the address of a site using a different DNS server
host example.com 8.8.8.8
# Using domain server:
# Name: 8.8.8.8
# Address: 8.8.8.8#53
# Aliases:
#
# example.com has address 93.184.216.34
# example.com has IPv6 address 2606:2800:220:1:248:1893:25c8:1946

# reverse query an IP address
host 93.184.216.34
# Host 34.216.184.93.in-addr.arpa. not found: 3(NXDOMAIN)

# query the address of a site
dig example.com
# ; <<>> DiG 9.10.3-P4-Ubuntu <<>> example.com
# ;; global options: +cmd
# ;; Got answer:
# ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 27151
# ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 2, ADDITIONAL: 3
#
# ;; OPT PSEUDOSECTION:
# ; EDNS: version: 0, flags:; udp: 4096
# ;; QUESTION SECTION:
# ;example.com.			IN	A
#
# ;; ANSWER SECTION:
# example.com.		27485	IN	A	93.184.216.34
#
# ;; AUTHORITY SECTION:
# example.com.		27485	IN	NS	b.iana-servers.net.
# example.com.		27485	IN	NS	a.iana-servers.net.
#
# ;; ADDITIONAL SECTION:
# a.iana-servers.net.	1072	IN	A	199.43.135.53
# b.iana-servers.net.	1072	IN	A	199.43.133.53
#
# ;; Query time: 237 msec
# ;; SERVER: 193.140.192.20#53(193.140.192.20)
# ;; WHEN: Sat Sep 23 17:33:00 +03 2017
# ;; MSG SIZE  rcvd: 136
#

# query the address of a site using a different DNS server
dig @8.8.8.8 example.com
# ; <<>> DiG 9.10.3-P4-Ubuntu <<>> @8.8.8.8 example.com
# ; (1 server found)
# ;; global options: +cmd
# ;; Got answer:
# ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 46070
# ;; flags: qr rd ra ad; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
#
# ;; OPT PSEUDOSECTION:
# ; EDNS: version: 0, flags:; udp: 512
# ;; QUESTION SECTION:
# ;example.com.			IN	A
#
# ;; ANSWER SECTION:
# example.com.		50153	IN	A	93.184.216.34
#
# ;; Query time: 37 msec
# ;; SERVER: 8.8.8.8#53(8.8.8.8)
# ;; WHEN: Sat Sep 23 17:34:27 +03 2017
# ;; MSG SIZE  rcvd: 56
#

# reverse query an IP address
dig -x 93.184.216.34
# ; <<>> DiG 9.11.3-1ubuntu1.9-Ubuntu <<>> -x 93.184.216.34
# ;; global options: +cmd
# ;; Got answer:
# ;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 36024
# ;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1
# 
# ;; OPT PSEUDOSECTION:
# ; EDNS: version: 0, flags:; udp: 65494
# ;; QUESTION SECTION:
# ;34.216.184.93.in-addr.arpa.	IN	PTR
# 
# ;; Query time: 1 msec
# ;; SERVER: 127.0.0.53#53(127.0.0.53)
# ;; WHEN: Sat Oct 19 22:51:33 +03 2019
# ;; MSG SIZE  rcvd: 55
# 

## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
## REMOTE CONNECTIONS
## ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# telnet (1) - user interface to the TELNET protocol
# nc (1) - arbitrary TCP and UDP connections and listens
# ssh (1) - OpenSSH SSH client (remote login program)
# scp (1) - secure copy (remote file copy program)
# rsync (1) - a fast, versatile, remote (and local) file-copying tool
# sshfs (1) - filesystem client based on ssh
# curl (1) - transfer a URL
# wget (1) - The non-interactive network downloader.

# create 'hello.html' file with content up to 'EOF'
cat << 'EOF' > hello.html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Title of the document</title>
</head>
<body>
Hello World!
</body>
</html>
EOF
# (creates the file)

# start an HTTP server on localhost
python -m SimpleHTTPServer
# Serving HTTP on 0.0.0.0 port 8000 ...
# 127.0.0.1 - - [23/Sep/2017 18:18:15] "GET /hello.html HTTP/1.1" 200 -
# 127.0.0.1 - - [23/Sep/2017 18:18:36] "GET /hello.html HTTP/1.1" 200 -

# make a GET request from localhost on port 8000
telnet localhost 8000
# Trying 127.0.0.1...
# Connected to localhost.
# Escape character is '^]'.
# GET /hello.html HTTP/1.1
#
# HTTP/1.0 200 OK
# Server: SimpleHTTP/0.6 Python/2.7.12
# Date: Sat, 23 Sep 2017 14:47:38 GMT
# Content-type: text/html
# Content-Length: 135
# Last-Modified: Sat, 23 Sep 2017 14:41:22 GMT
#
# <!DOCTYPE html>
# <html>
# <head>
# <meta charset="UTF-8">
# <title>Title of the document</title>
# </head>
# <body>
# Hello World!
# </body>
# </html>
# Connection closed by foreign host.

# make a GET request from localhost on port 8000
printf "GET /hello.html HTTP/1.1\r\n\r\n" | nc localhost 8000
# HTTP/1.0 200 OK
# Server: SimpleHTTP/0.6 Python/2.7.12
# Date: Sat, 23 Sep 2017 15:18:36 GMT
# Content-type: text/html
# Content-Length: 135
# Last-Modified: Sat, 23 Sep 2017 14:41:22 GMT
#
# <!DOCTYPE html>
# <html>
# <head>
# <meta charset="UTF-8">
# <title>Title of the document</title>
# </head>
# <body>
# Hello World!
# </body>
# </html>

# start an SMTP server on localhost
python -m smtpd -n -c DebuggingServer localhost:8025
# ---------- MESSAGE FOLLOWS ----------
# Body of email.
# ------------ END MESSAGE ------------
# ---------- MESSAGE FOLLOWS ----------
# Body of email.
# ------------ END MESSAGE ------------

# send an email to localhost on port 8025
telnet localhost 8025
# Trying 127.0.0.1...
# Connected to localhost.
# Escape character is '^]'.
# 220 ext Python SMTP proxy version 0.2
# HELO host.example.com
# MAIL FROM:<user@host.example.com>
# RCPT TO:<user2@host.example.com>
# DATA
# Body of email.
# .
# QUIT
# 250 ext
# 250 Ok
# 250 Ok
# 354 End data with <CR><LF>.<CR><LF>
# 250 Ok
# 221 Bye
# Connection closed by foreign host.

# send an email to localhost on port 8025
printf "HELO host.example.com\r
MAIL FROM:<user@host.example.com>\r
RCPT TO:<user2@host.example.com>\r
DATA\r
Body of email.\r
.\r
QUIT\r\n" | nc localhost 8025
# 220 ext Python SMTP proxy version 0.2
# 250 ext
# 250 Ok
# 250 Ok
# 354 End data with <CR><LF>.<CR><LF>
# 250 Ok
# 221 Bye

# listen port 1234 and send 'hello.html'
nc -l 1234 < hello.html
# (listens the port)

# connect localhost on port 1234
nc localhost 1234
# <!DOCTYPE html>
# <html>
# <head>
# <meta charset="UTF-8">
# <title>Title of the document</title>
# </head>
# <body>
# Hello World!
# </body>
# </html>

# connect to 'example.com' as 'user' using SSH
ssh user@example.com
# (connects to the server)

# transfer 'hello.html' file to the remote server using SSH
scp hello.html user@example.com:~
# (transfers the file)

# transfer 'hello.html' file from the remote server using SSH
scp user@example.com:~/hello.html .
# (transfers the file)

# transfer 'hello.html' file to the remote server using SSH
rsync hello.html user@example.com:~
# (transfers the file)

# transfer 'hello.html' file from the remote server using SSH
rsync user@example.com:~/hello.html .
# (transfers the file)

# create an empty directory for mounting
mkdir mnt
# (creates the directory)

# mount home directory of the remote server to 'mnt' directory
sshfs user@example.com:/home/user mnt
# (mounts the directory)

# unmount 'mnt' directory
fusermount -u mnt
# (unmounts the directory)

# display 'example.com' page
curl example.com
# <!doctype html>
# <html>
# <head>
# ...
# </head>
#
# <body>
# <div>
#     <h1>Example Domain</h1>
#     <p>This domain is established to be used for illustrative examples in documents. You may use this
#     domain in examples without prior coordination or asking for permission.</p>
#     <p><a href="http://www.iana.org/domains/example">More information...</a></p>
# </div>
# </body>
# </html>

# download 'Alice in Wonderlands' book in text format and save as 'alice.txt'
wget http://www.gutenberg.org/files/11/11-0.txt -O alice.txt
# (downloads the file)

```

Related:

* <https://gokcehan.github.io/>

Tags:

    #linux #commands #network #networkManagement #cheatsheet
