# Creating Bonding Network Interface with `nmcli`

Creating a configuration for a bonding interface with nmcli involves a few steps. Minimal steps are:

1. Identify adapters: Use `nmcli device status`
1. Create bonding device: Use `nmcli connection add`
1. Attach interface to the bond: Create a connection from the adapter to the bond device using `nmcli connection add`
1. Set the bond adapter online: Issue an nmcli connection up command for the bond adapter
1. Reboot: Although the reboot is not absolutely required, it is strongly recommended. A reboot will clean up any configuration fragments left around

```
nmcli connection add type bond \
     con-name bond0 \
     ifname bond0 \
     bond.options "mode=active-backup,miimon=1000"
nmcli connection add type ethernet slave-type bond \
     con-name bond0-port1 \
     ifname enp0s20u1u1 \
     master bond0
nmcli connection add type ethernet slave-type bond \
     con-name bond0-port2 \
     ifname enp0s20u1u2 \
     master bond0
nmcli connection up bond0
nmcli device status
reboot
nmcli device status
```

For more information on nmcli, check the [nmcli documentation][docs].

[docs]: <https://wiki.gnome.org/Projects/NetworkManager/SystemSettings/>

Related:

* https://www.kernel.org/doc/Documentation/networking/bonding.txt

Tags:

    #linux #sysadmin #networking #wlan #ethernet #interfaces #devices
