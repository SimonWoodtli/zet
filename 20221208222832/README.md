# Dietpi with Adguard and Unbound on Raspberry Pi Zero V1

> 📝 Unbound is a reverse DNS. It allows you to have a local DNS server that will be used instead of a public one like Cloudflare, Google etc. which increases privacy. However for first time load time of websites it will be a bit slower.

## 1. Flash DietPi with Balena Etcher

1. select pi zero and download the latest version of [DietPi] 
1. extract 7z
1. flash image with balena etcher on your sd card
1. remount the sd disk. Check mountpoint: `lsblk` and cd into the first partition of the sd card.
1. change file: `vi dietpi.txt`

```bash
AUTO_SETUP_NET_ETHERNET_ENABLED=0
AUTO_SETUP_NET_WIFI_ENABLED=1
```
6. change file: `vi dietpi-wifi.txt`. Make sure to use 2.4Ghz as the Pi Zero doesn't support 5Ghz

```bash
aWIFI_SSID[0]=’WLAN_Name‘
aWIFI_KEY[0]=’WLAN_Passwort‘
```

## 2. Finish First Boot Install

1. put the sd card into your pi and boot it up
1. ssh into the pi with credentials: user: root pw: dietpi 
1. let first boot proceed
    1. change user/root pws
    1. go to bottom of menu 'install' and install minimal dietpi. It will reboot the pi after it's done.
1. create static ip: Either use your Router to do this or do it on you Pi directly.
    1. goto `dietpi-config` 'Network Options: Adapters' 'WIFI' and change 'DHCP' to 'Static IP'
    1.'Copy current address to static'.
    1. Now change 'Static IP' and set it to an IP which is outside of your Routers DHCP range. Look up on your Router what your DHCP range is.
    1. 'Apply changes'
1. kill window and ssh again into Pi with new IP then `reboot` it.

## 3. Change settings

1. Change some configs: `dietpi-config`
    1. hostname: 'Security Options'
    1. Location: 'Language/Region'
    1. reboot your pi

## 4. Install Adguard

1. install software: `dietpi-software` 
    1. search for 'Adguard' install it
    1. enable Unbound 
    1. cancel on Static IP
    1. goto bottom menu 'Install'

## 5. Configure Adguard on Browser

> 🧐 If you plan to expose your Pi directly to the internet or setup Adguard on a remote VPS you should setup https. Go to 'Settings' 'Encryption Settings'. You should also setup 2FA with Authelia if you plan to do so and other general security measurments like ssh hardening: disable pw access ..., fail2ban etc.

> 🧐 If you want custom filter settings for a client: 'Settings' 'Client Settings'

> 🧐 If you don't have a DHCP server, or want to replace the one from your router with Adguard: 'Settings' 'DHCP Settings'

1. Go to your web browser and login:
    1. URL: http://<your.IP>:8083
    1. User: admin Pw: <yourGlobalSoftwarePassword>
1. Go to 'Settings' 'General Settings'
    1. enable parental control
    1. enable safe search
    1. enable anonymize client IP
    1. keep logs only 24 hours
    1. keep statistics 24 hours
1. Go to 'Filter' 'DNS Blocklist'
    1. Choose from list 'Adaway Blocklist', 'Dan Pollock's List'
    1. Add a [custom list]: copy any url you want to try and add it

## 6. Router Settings (if you choose all clients to use Adguard)

1. Login on Adguard Interface go to 'Setup Guide': Either you choose to setup each device individually or you set it up globally on your router
1. Login to your Router: 
    1. Go to IPV4 Settings and 'Use custom DNS Server' write the static IP set for your pi0. It can also be under DHCP settings or Custom DNS Server settings. Just enable 'manual' DNS and put your dietpi's IP in it.
    1. Problem IPV6 is not setup to use DNS from Adguard. So it will use DNS from ISP and that means any IPV6 site/ad won't get blocked. 
    1. Solution 1:
    1. Go to IPV6 settings activate 'Router Advertisment' and set ULA to 'fd00' leave the rest of the fields empty. Now restart your dietpi
    1. In your Adguard interface in 'Setup Guide' there should now be an ipv6 adress that starts with fd. Copy it
    1. On your routers IPV6 settings enable IPV6-prefix
    1. On your routers IPV6 settings enable DNSv6-Server. And now paste the 'fd00 xxx' IPV6 from Adguards 'Setup Guide' into the field.
    1. Solution 2: just deactivate IPV6 on your router altogether and be done with it. If your router can deactivate IPV6 this is the better option IMO.


After everything create a backup of you SD card.


Related:

[custom list]: <https://firebog.net/>
[DietPi]: <https://dietpi.com/>

Tags:

    #linux #raspberryPi #adguard #adblock #project #dietPi
