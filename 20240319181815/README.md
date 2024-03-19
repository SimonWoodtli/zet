# Network Manager

> 🧐 While Network Manager still uses configuration files, it is usually best to
>rely on its various utilities for manipulating and updating them. Network
>Manager should be almost the same on different systems.

Network Manager can use the traditional network files from several
distributions. The networking profiles are supported by plugins. The key-value
is the preferred file format.

Modern systems often have dynamically changing configurations. For this Network
Manager provides a distro-independent way to configure your network.

* Networks may change as a device is moved from place to place
* Wireless devices may have a large choice of networks to hook into
* Devices may change as hardware, such as wireless devices, are plugged in or turned on and off

## Commands

* Interactive TUI tool: `nmtui`
* CLI tool for scripting: `nmcli`

## History

Once upon a time, network connections were almost all wired (Ethernet) and did
not change unless there was a significant change to the system.

As a system was booted, it consulted the network configuration files in the
/etc directory subtree in order to establish the interface properties such as
static or dynamic (DCHP) address configuration, whether the device should be
started at boot, etc.

If there were multiple network devices, policies had to be established as to
what order they would be brought up, which networks they would connect to, what
they would be called, etc.

As wireless connections became more common (as well as hotplug network devices
such as on USB adapters), configuration became much more complicated, both
because of the transient nature of the hardware and that of the specific
networks being connected to.

## Plugins

The Network Manager has several optional plugins for traditional configuration compatibility:

* ifupdown for /etc/network/interfaces
* ifcfg-rh for /etc/sysconfig/network-scripts
* ifcfg-suse is for simple compatibility for SUSE and openSUSE
* key-file is a generic replacement for system specific configuration files

There is a configuration option in /etc/NetworkManager/NetworkManager.conf in
the [main] section that lists which plugins for configuration processing are to
be used in a comma-separated list. For more details, see the
[NetworkManager.conf documentation][docs].

[docs]: <https://developer-old.gnome.org/NetworkManager/stable/NetworkManager.conf.html>

Related:

* [20221214213533](/20221214213533/) Networking: Useful Commands to Troubleshoot and Manage network interfaces and DNS on Linux
* [20240319175545](/20240319175545/) Network Devices/Interfaces: Names and labels explained
* [20240319181218](/20240319181218/) Network Devices/Interfaces: Common config file locations

Tags:

    #linux #sysadmin #networking #wlan #ethernet #interfaces #devices
