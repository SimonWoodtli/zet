# Network Devices/Interfaces: Names and labels explained

Unlike block and character devices, network devices are not associated with
special device files, also known as device nodes. Rather than having associated
entries in the /dev directory, they are known by their names.

* eth0, eth1, eno1, eno2, etc., for ethernet devices.
* wlan0, wlan1, wlan2, wlp3s0, wlp3s2, etc., for wireless devices.
* br0, br1, br2, etc., for bridge interfaces.
* vmnet0, vmnet1, vmnet2, etc., for virtual devices for communicating with virtual clients.

## Naming Convention Issue

Historically, multiple virtual devices could be associated with single physical
devices; these were named with colons and numbers; so, eth0:0 would be the
first alias on the eth0 device. This was done to support multiple IP addresses
on one network card. However, with the use of `ip` instead of `ifconfig`, this
method is deprecated and we will not pursue it. This is not compatible with
IPv6.

The historical device naming conventions encountered difficulties, particularly
when multiple interfaces of the same type were present. For example, suppose
you have two network cards; one would be named eth0 and the other eth1.
However, which physical device should be associated with each name?

The simplest method would be to have the first device found be eth0, the second
eth1, etc. Unfortunately, probing for devices is not deterministic for modern
systems, and devices may be located or plugged in an unpredictable order. Thus,
you might wind up with the Internet interface swapped with the local interface.
Even if hardware does not change, the order in which interfaces are located has
been known to vary with kernel version and configuration.

Many system administrators have solved this problem in a simple manner, by
hard-coding associations between hardware (MAC) addresses and device names in
system configuration files and startup scripts. While this method has worked
for years, it requires manual tuning and had other problems, such as when MAC
addresses were not fixed; this can happen in both embedded and virtualized
systems.

## Solution

The Predictable Network Interface Device Names (PNIDN) is connected to udev and
integration with systemd. There are now 5 types of names that devices can be
given:

* Incorporating Firmware or BIOS provided index numbers for onboard devices
    * Example: eno1
* Incorporating Firmware or BIOS provided PCI Express hotplug slot index numbers
    * Example: ens1
* Incorporating physical and/or geographical location of the hardware connection
    * Example: enp2s0
* Incorporating the MAC address
    * Example: enx7837d1ea46da
* Using the old classic method
    * Example: eth0

## Naming Explained with examples

Output of: `ip link show | grep enp`

```
2: enp4s2: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc pfifo_fast state DOWN mode DEFAULT qlen 1000
3: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
```

Output of: `ifconfig | grep enp`

```
enp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500
enp4s2: flags=4099<UP,BROADCAST,MULTICAST mtu> 1500
```

These names are correlated with the physical locations of the hardware on the
PCI system (command and output below):

Output of: `lspci | grep Ethernet`

```
02:00.0 Ethernet controller: Marvell Technology Group Ltd. 88E8056 PCI-E Gigabit Ethernet Controller (rev 12)
04:02.0 Ethernet controller: Marvell Technology Group Ltd. 88E8001 Gigabit Ethernet Controller (rev 14)
```

The triplet of numbers at the beginning of each line from the lspci output is
the bus, device (or slot), and function of the device; hence it reveals the
physical location.

Likewise, for a wireless device that previously would have been simply named
wlan0 (commands and outputs below):

Output of: `ip link show | grep wl`

```
3: wlp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DORMANT qlen 1000
```

Output of: `lspci | grep Centrino`

```
03:00.0 Network controller: Intel Corporation Centrino Advanced-N 6205 [Taylor Peak] (rev 34)
```


Related:

* [20240319181218](/20240319181218/) Network Devices/Interfaces: Common config file locations

Tags:

    #linux #sysadmin #networking #wlan #ethernet #interfaces #devices
