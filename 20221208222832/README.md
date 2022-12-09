# Dietpi with Adguard on Raspberry Pi Zero V1

1. download latest version of dietpi 'DietPi_RPi-ARMv8-Bullseye.7z': https://dietpi.com/downloads/images/
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

7. put the sd card into your pi and boot it up
8. ssh into the pi with credentials: user: root pw: dietpi 
9. 
