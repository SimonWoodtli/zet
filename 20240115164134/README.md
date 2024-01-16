# LAN Server Service: Setup USB Sharing over Network

> 📝 virtualHere client CLI only works with paid version (and not sure if I
> want that). But the whole project gives a weird outdated feeling. ./CLI -t
> “HELP” what’s this? Modern CLI: ./CLI help, or at least unix way ./CLI
> --help|-h
> Alternative: https://wiki.archlinux.org/title/USB/IP

## Scripts explained

All these scripts do is make it more convenient to add your usb devices to the
server. Or make them available on the client. The scripts still need some
optimizations but OK for now. They just run a script which gets triggered on
bootup via a systemctl service and triggered again when you shut down the
machine.

I use this mostly for VMs (they can be on any machine). My rpi acts as a server
and any machine with a VM on it can access these usb devices as a client.

Note that only one machine can access the usb stick at a time.

### Server Setup

1. Clone my dotfiles
1. Run my client setup script as root
1. Edit usbipserver file: `sudo vi /usr/sbin/usbipserver`: Change the usbIds
   value to your own USBs you want to share

* <https://github.com/SimonWoodtli/dotfiles/tree/main/scripts/__usbip/server>

### Client Setup

1. Clone my dotfiles
1. Run my client setup script as root
1. Edit usbipserver file: `sudo vi /usr/sbin/usbipserver`: Change the usbIds
   value to your own USBs you want to attach/detach from server and change the
   local server IP.

* <https://github.com/SimonWoodtli/dotfiles/tree/main/scripts/__usbip/client>

## Useful commands (new zet)

TODO Create a new zet for cmds:

* List all devices: `usbip list -l`

Related:

* <https://derushadigital.com/other%20projects/2019/02/19/RPi-USBIP-ZWave.html>
* <https://wiki.archlinux.org/title/USB/IP>

Tags:

      #linux #server #homelab #networking #usb #devices #sharing
