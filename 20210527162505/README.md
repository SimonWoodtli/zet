# How to scan LAN for device IPs?

To get a scan with mac-address and ip: `sudo nmap -sT -O 192.168.1.0/24` or `sudo nmap -sP 192.168.1.0/24` or `sudo nmap -A -T4 [SPECIFIC_IP]`

To get a scan with mac-address and ip: 1. `ifconfig` and cp network interface 2. `sudo arp-scan --interface=enp37s0 --localnet`

My fav: `sudo nmap -sT -O 192.168.1.0/24` and then use `mac [OUI]` (first 6char of mac-address to get info about vendor)
