# What are some common commands for ss?

socket statisitcs/netstat tool:

[checkout](https://computingforgeeks.com/netstat-vs-ss-usage-guide-linux/) for more info
–n, –numeric don’t resolve service names
-r, –resolve : resolve host hostnames.
-l, –listening display listening sockets
-o, –options show timer information
-e, –extended show detailed socket information
-m, –memory show socket memory usage
-p, –processes show process using socket
–s, –summary show socket usage summary
-N, –net switch to the specified network namespace name
-4, –ipv4 display only IP version 4 sockets
-6, –ipv6 display only IP version 6 sockets
–0, –packet display PACKET sockets
-t, –tcp display only TCP sockets
-S, –sctp display only SCTP sockets
-u, –udp display only UDP sockets
-w, –raw display only RAW sockets
-x, –unix display only Unix domain sockets
-f, –family=FAMILY display sockets of type FAMILY
