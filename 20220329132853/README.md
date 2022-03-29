# Anonymous VM with Tor connection via Whonix-Gateway and any Linux distro

The idea is to use the Whonix-Gateway virtual machine, which creates a tor 
connection. Together with any Distro of your liking and funnel the traffic 
through the Gateway to get a tor connection on the whole VM.

* To change IP some distros need to change settings here: `/etc/network/interfaces`

## Kali Linux

1. Make sure to download and import the Whonix-Gateway-CLI image
2. Download the .ova Kali image and import into VirtualBox
3. Update both VMs and change passwords
4. Change the VirtualBox settings from NAT to Internal Network 'Whonix'
5. Right click the Network Icon in top panel to edit IP4:

```
static ip: 10.152.152.11
subnet mask: 255.255.192.0
gateway: 10.152.152.10 # same as Whonix-Gateway
DNS: 10.152.152.10
```

6. Disconnect the Network and reconnect it.
7. Check your IP: https://check.torproject.org/ 
8. Use script to turn on/off: `kali` and `kali off`

## Backbox

1. Make sure to download and import the Whonix-Gateway-CLI
2. Download [Blackbox] and setup a new VM in VirtualBox

**Requirements**
* 4GB RAM
* 3 cores
* 128MB Video RAM, VBoxSVGA and 3D acceleration
* 30GB HDD

3. Start Blackbox and Whonix-Gateway update both systems and change passwords
4. Change the VirtualBox settings from NAT to Internal Network 'Whonix'
5. In Backbox click on network icon in top panel and edit => IP4 settings tab:

```
static ip: 10.152.152.11
subnet mask: 255.255.192.0
gateway: 10.152.152.10 # same as Whonix-Gateway
DNS: 10.152.152.10
```

6. Disconnect the Network and reconnect it.
7. Check your IP: https://check.torproject.org/ 
8. Use script to turn on/off: `backbox` and `backbox off`
