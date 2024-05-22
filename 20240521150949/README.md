# Server Service: Add netboot.xyz

Adding this program makes using Ventoy or flashing USB sticks to install OS
obsolete. This will allow you to fetch OS images over the network.

## Requirements

* static IP on your server
* an actual server
* access to DHCP server via router or pihole/unbound or whatever your setup (to configure it to pixie boot into netboot.xyz)

## Install via Docker

1. ssh into your server
1. `cd ~/Containers` (or wherver you have your docker c. stacks)
1. `mkdir -p netboot_xyz/{assets,config}`
1. Append the docker-compose config from the [linuxserver.io][config] image to your existing docker-compose file. Or create a new one if you have no docker containers running on your server.
1. comment the line with env. var '- MENU_VERSION=1.9.9 ' to get the latest version.
1. adjust the volumes path to './netboot_xyz/assets' and './netboot_xyz/config'.
1. run it `docker compose up -d`
1. Check if it is running `docker ps` and `docker logs netbootxyz`
1. If you have an internal domain name and want to reverse proxy to '192.xx:3000' to access the webapp go for it (optional)

## Configure your DHCP

This depends on who controls DHCP in your network. Usually it's your router.

With OpenWRT:

1. ssh into router
1. Run this: (adjust the 192.xx IP to whatever your server that runs netbootxyz container)

```
tee -a /etc/dnsmasq.conf << EOF
dhcp-match=set:ipxeclient,60,IPXEClient*
dhcp-match=set:bios,60,PXEClient:Arch:00000
dhcp-boot=tag:bios,netboot.xyz.kpxe,,192.168.8.99
dhcp-match=set:efi32,60,PXEClient:Arch:00002
dhcp-boot=tag:efi32,netboot.xyz.efi,,192.168.8.99
dhcp-match=set:efi32-1,60,PXEClient:Arch:00006
dhcp-boot=tag:efi32-1,netboot.xyz.efi,,192.168.8.99
dhcp-match=set:efi64,60,PXEClient:Arch:00007
dhcp-boot=tag:efi64,netboot.xyz.efi,,192.168.8.99
dhcp-match=set:efi64-1,60,PXEClient:Arch:00008
dhcp-boot=tag:efi64-1,netboot.xyz.efi,,192.168.8.99
dhcp-match=set:efi64-2,60,PXEClient:Arch:00009
dhcp-boot=tag:efi64-2,netboot.xyz.efi,,192.168.8.99

EOF
```

Take any device and boot from network should now start up netbootxyz 🎉

## Serve OSs Local vs Remote

webapp: http://myserversIP:3000

If you want you may download all the images you want to your server and then
netbootxyz won't download them each time. However that requires you to change
settings in 'boot.conf' on the webapp and point the server away from the github
remote to your local servers IP. 

Why I don't choose to do so? Because that means you can only serve images from
your server and need to download all of them. If you try to select an image
that isn't on your server it will error out.


[config]:<https://docs.linuxserver.io/images/docker-netbootxyz/#usage>

Related:

* https://forum.openwrt.org/t/uefi-pxe-boot-server-on-openwrt/179968/2
* https://forum.openwrt.org/t/dnsmasq-pxe-boot-using-netboot-xyz/56075/2
* https://docs.linuxserver.io/images/docker-netbootxyz

Tags:

    #linux #server #docker #network #PXE
