# Networking Problem Troubleshooting

Network problems can be caused either by software or hardware, and can be as
simple as is the device driver loaded, or is the network cable connected. If
the network is up and running but performance is terrible, it really falls
under the banner of performance tuning, not troubleshooting.

The following items need to be checked when there are issues with networking:

* IP configuration: Use `ifconfig` or `ip` to see if the interface is up, and
  if so, if it is configured
* Network Driver: If the interface cannot be brought up, maybe the correct
  device driver for the network card(s) is not loaded. Check with `lsmod` if
  the network driver is loaded as a kernel module, or by examining relevant
  pseudofiles in /proc and /sys, such as /proc/interrupts or /sys/class/net.
* Connectivity: Use `ping` to see if the network is visible, checking for
  response time and packet loss. `traceroute` can follow packets through the
  network, while `mtr` can do this in a continuous fashion. Use of these
  utilities can tell you if the problem is local or on the Internet.
* Default gateway and routing configuration: Run `route -n` and see if the
  routing table make sense
* Hostname resolution: Run `dig` or `host` on a URL and see if DNS is working
  properly.

Tags:

    #linux #sysadmin #LFS207 #networking #internet
