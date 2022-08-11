# Connect/power rpi4 with iPad via usb-c

1. Download the rpi imager software
2. Flash a micro sd with rpi OS lite 64bit, make sure to set hostname, ssh, username/pw and wifi, disable telemetry
3. Remount the disk after successful flash 
  1. edit /run/media/boot/config.txt 
  2. last line add: `dtoverlay=dwc2,dr_mode=peripheral`
  3. edit /run/media/boot/cmdline.txt
  4. before `rootwait` add `modules-load=dwc2,g_ether`

* This two changes will enable USB OTG peripheral mode. So the pi acts as a USB peripheral rather than a USB host. And it sets the peripheral type to be Ethernet.
* USB OTG can be used on USB A as well. However the Pi only supports it on the USB C port.

1. Connect iPad with Pi via USB C
2. Wait until Ethernet pops up in the iPad's settings
3. Open Terminal App "Blink" and SSH into your Pi with the hostname.local
4. Activate Zero Conf on pi via Avahi 
  1. Check Avahi status: `sudo systemctl status avahi-daemon`
  2. Check Avahi Conf `sudo vi /etc/avahi/avahi-daemon.conf` \# allow the eth0 interface or deny eth1 interface?


https://www.youtube.com/watch?v=3UPaI4Hp66Y
