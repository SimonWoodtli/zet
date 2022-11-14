# Connect/Power Rpi4 with iPad over USB-C

## Install OS: Rpi

> 📝 never tested, just theory

1. Download the rpi imager software
2. Flash a micro sd with rpi OS lite 64bit make sure to set hostname, ssh, username/pw and wifi, disable telemetry
3. Remount the disk after successful flash 
    1. edit /run/media/boot/config.txt 
    2. last line add: `dtoverlay=dwc2,dr_mode=peripheral`
    3. edit /run/media/boot/cmdline.txt
    4. before `rootwait` add `modules-load=dwc2,g_ether`

* This two changes will enable USB OTG peripheral mode. So the pi acts as a USB peripheral rather than a USB host. And it sets the peripheral type to be Ethernet.
* USB OTG can be used on USB A as well. However the Pi only supports it on the USB C port.

1. Connect iPad with Pi via USB-C
2. Wait until Ethernet pops up in the iPad's settings
3. Open Terminal App "Blink" and SSH into your Pi with the hostname.local
4. Activate Zero Conf on pi via Avahi 
    1. Check Avahi status: `sudo systemctl status avahi-daemon`
    2. Check Avahi Conf `sudo vi /etc/avahi/avahi-daemon.conf` \# allow the eth0 interface or deny eth1 interface?

## Install OS: Ubuntu

1. Download/Install the rpi imager software
2. Flash a Micro SD with Ubuntu server 64bit make sure to set hostname and username (wifi/ssh works only on rpi OS)

Problem: only username and hostname applied, wifi and ssh did not

> ⚠️  Editing the files after flash directly did not work. Booting up the PI connected to monitor/keyboard first to configure is required, since the wifi config did not work either.

### Config and Setup

#### Enable IP assignment from Pi automatically:

1. remount Micro SD card into your computer
1. create netplan file: `sudo vim /run/media/foo/etc/netplan/20-rpi.yaml`

```yaml
network:
  version: 2
  wifis:
    renderer: networkd
    wlan0:
      dhcp4: true
      optional: true
      access-points:
        "TheNameofWifi/SSID":
          password: "yourWifiPW"
  ethernets:
    usb0:
      link-local: [ ipv4 ]
```

1. apply netplan: `sudo netplan apply` (can't do on mounted sd)
1. unmount and put sd card into pi

If this works the ethernet in the ipads settings assigns an ip address to the pi.

### First Boot 

#### Enable Ethernet over USB-C:

1. edit: `sudo vim /boot/firmware/config.txt`
1. last line add: `dtoverlay=dwc2,dr_mode=peripheral`
1. edit `sudo vim /boot/firmware/cmdline.txt`
1. before `rootwait` add `modules-load=dwc2,g_ether`

If this works the Ethernet Interface pops up in the ipads settings.

#### Enable SSH

1. Power Up your Pi with a monitor and keyboard connected to it.
1. check all the changed settings add the /boot config files lines again (they don't apply after first try)
1. check if really not active/enabled: `sudo systemctl status ssh`
1. if not enable service: `sudo systemctl enable --now ssh`
1. add 'open port 22' to firewall rules: `sudo ufw allow ssh`
1. uncomment "PasswordAuthentication" `sudo vim /etc/ssh/shhd_config` 
1. set PasswordAuth to yes `sudo vim /etc/ssh/sshd_config/60-cloudimg-settings.conf"`

If this works you can use iSh to ssh into pi without wifi/router

## Login Process/Usage

1. at home/with my router: use the hostname from within iShs ~/.ssh/config with a static IP for the Pi. This will use the Wifi IP but the connection is fast enough.
1. any other situation: use `blinks` terminal app and `ssh sero@hostname.local` then `ip a` remember the usb0 169.xx IP. Then use `iSh` with this IP (It's a different IP to the one that is in the iPads Ethernet settings). 

> ⚠️  It would be great if I could directly use `iSh` but the hostname.local ssh doesn't work. So getting the Pi's usb0 IP is required and can only be done with logging in first. Solution just create a static IP on your home network for the PI and create a ~/.ssh/config file with a hostname and that static IP to use.

* If I use the 169.xx IP from the ethernet ipad settings: it throws a port error connection refused.

### Figure out

Another way would be to limit the range of the 169.xx Ips to be set to only two one for the ethernet settings ipad and one for the pi. That way It'd be easy to guess the IP. So the random IP assignment would fall to only these two IPs.

Related:

* <https://www.youtube.com/watch?v=3UPaI4Hp66Y>

Tags:
