# Install & setup Pi-hole

Pihole place: /etc/pihole (install log)
Pihole ip: 192.168.0.17
Pihole pw: o7geWdeq
Webinterface: http://pi.hole/admin or http://192.168.0.17/admin

Restart pi-hole if setting new ip

—

**important** Tutorial: https://www.derekseaman.com/2019/09/how-to-pi-hole-plus-dnscrypt-setup-on-raspberry-pi-4.html

**additional info maybe if vpn need to be on top** https://www.itchy.nl/raspberry-pi-4-with-openvpn-pihole-dnscrypt/

DNScrypt servers: https://dnscrypt.info/public-servers/
Relay servers: https://github.com/DNSCrypt/dnscrypt-resolvers/blob/master/v2/relays.md
How anonymized DNS works (dont use any steps in here!): https://cryptostorm.is/blog/anondns

—

1. Writing The Raspbian Lite to your MicroSD card
  1. Download the latest version of Raspbian Lite.I got the 'desktop and recommended software' package. Do NOT unzip it.

  2. Download and install ​Etcher.io, which we will use to write the Raspbian lite image to the SD card. There are both PC and Mac versions.

  3. Connect your card reader and insert the microSD card. Warning: contents will be over-written!

  4. Start Etcher, click "Select Image" and find the Raspbian Lite zip file you downloaded.

  5. Click "Flash!" and wait for the zip to be written to the memory card and the validation to complete. If an error occurs, make sure the card reader/card is not locked. If it's not locked, possibly the download is corrupted or not complete. Try and re-download the Raspbian Lite zip.

  6. (Skip some parts) connect pi to power with keyboard&monitor startup
  $ sudo raspi-config

  configure keyboard time zone wifi activate ssh

  7. $ sudo apt install dnsutils (needed for $ nslookup google.com)
  Nslookup shows at the top the dns-server that is used
  Also dns lookup: $ cat /etc/resolv.conf

  $ ifconfig (local ip, mac adresse) $ ip addr show
  MAC-adress e.g. b6:27:eb:c7:29:20 // ip local e.g: 192.168.0.3

  $ curl ifconfig.me (external ip)

2. Powering Up and Configuring your Raspberry Pi
  1. Connect your Raspberry Pi to a good power source. Since there's no power switch, it will start to immediately boot (unless you bought the kit I linked to from Amazon, where the power supply has an on/off switch). If you have a monitor and keyboard attached when booting the first time, a nice GUI wizard will appear to walk you through configuration of items such as locale, keyboard, timezone, new password, software updates, etc. I prefer this method over raspi-config (next step). Also take note of the IP assigned, which the wizard will show as well.
  2. If you are doing the setup 'headless', wait a couple of minutes for the system to boot. If you are using the Eero wifi mesh system like I am, you should get a notification that a new device joined the network. Open the Eero app and take note of the IP address the Raspberry Pi was assigned. If you are not using Eero, you can find the IP via other means. That includes connecting a monitor/keyboard to the Raspberry Pi, looking at your router for a new device, or using a network scanning app like "InetTools" for iOS using the LAN scan feature  to look at before/after maps to determine what's new. Or if you get lucky, you can open a terminal and type ping raspberry.pi and see if it responds.

  3. SSH into the Raspberry Pi as user 'pi' and open the configuration tool (default password is raspberry):

  ssh pi@RaspberryIP
  sudo raspi-config

  4. At a minimum consider configuring the following items with the tool. If you ran through the configuration with the desktop GUI using the keyboard and monitor, most of this will have already been done.

  Change password (menu 1): very important
  Network options (menu 2): Change hostname (optional)
  Boot options (menu 3): console autologin (optional, bad for security, good for ease of use)
  Localisation options (menu 4): keyboard layout, timezone (important)
  Interface options (menu 5): Enable ssh
  5. Next we should configure the Raspberry Pi for a static IP address. You can do this two ways. One, create a reservation in your router (which I did), or we can configure a static IP directly in the RPi. If you don't want to go the router route, enter the following command:

  sudo nano dhcpcd.conf

Uncomment the line under # Example static IP configuration and fill in the proper IPs. You don't need an IPv6 address, so that line can be left commented. Save the configuration file and exit nano.

**give static ip via router** i used this one
Now that we've found our Raspberry Pi's IP address + MAC Address, we need to assign it an INTERNAL/LOCAL static IP address.
This process is going to vary wildly based on which router/DHCP server you use, so we'd recommend Googling your router's model name/number (can be found on the back) + "how to set static IP" (ex: "Netgear R7000 how to set static ip").
If you're willing and somewhat tech savvy, you might also be able to figure it out on your own.
Start by navigating to your router's admin page. The IP for this is typically located on a sticker on the back of your ISP's provided router (along with the admin page's default username and password), but you can also find it by running the command "ipconfig" in command prompt on a Windows PC. Your router's IP will be listed after "default gateway" (https://i.imgur.com/S2Ndc0w.png)
Log in to the admin page either with the Iogin credentials listed on the back of the router, or by googling the model number of the router along with "default password". Some routers use a randomly generated default password, so googling will not work for those.
Once logged in, look for a tab labeled "DHCP Reservation", "Static IP Assignment", or something along those lines. (https://i.imgur.com/FeMjd4V.png) You may have to go to the Advanced menu to access this. (https://i.imgur.com/6l4kIqH.png)
Enter the MAC address we grabbed earlier with Angry IP scanner, and then enter/select your desired static IP address (make sure you're using something not taken by another device on your network). (https://i.imgur.com/znUTbKv.png)
Hit Apply (or whatever the equivalent is for your router)

**or if you prefer give pi static ip** details are here too
$ sudo nano /etc/dhcpcd.conf

network configuration
  6. Now we need to update all of the packages, if you haven't done so during the desktop GUI configuration process. Type:

sudo apt-get update
sudo apt-get dist-upgrade

Wait for the updates to complete. I like to reboot after the updates, so type sudo reboot.

  7. To be more secure and get automated updates, we will install unattended-upgrades.

sudo apt-get install unattended-upgrades

sudo nano /etc/apt/apt.conf.d/50unattended-upgrades

Add the following two lines just after the origin-Debian section, and comment out the debian lines.

"origin=Raspbian,codename=${distro_codename},label=Raspbian";
"origin=Raspberry Pi Foundation,codename=${distro_codename},label=Raspberry Pi Foundation";

auto-updates
Now we need to edit another file:

sudo nano /etc/apt/apt.conf.d/20auto-upgrades

Delete the existing lines and paste this in:

APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::Unattended-Upgrade "1";
APT::Periodic::Verbose "1";
APT::Periodic::AutocleanInterval "7";

To enable unattended updates type:

sudo dpkg-reconfigure --priority=low unattended-upgrades

5. Update Raspberry Pi 4 EEPROM (Firmware)

From time to time new Raspberry pi 4 EEPROM (firmware) may be available. This procedure will install an auto-updater and keep you on the latest firmware version. Run these commands below. If it says update required, then merely reboot your RPI with sudo reboot and the update will be installed.

sudo apt update
sudo apt full-upgrade
sudo apt install rpi-eeprom
sudo rpi-eeprom-update

Raspberry pi boot eeprom
  6. Package Removal

Wolfram and Libreoffice are both space hogs on the Raspberry Pi 4, and likely you won't need them. Plus this makes your backups larger, so let's get rid of them. This will save over 1GB of space. If you want them, please skip this section.

sudo apt-get remove --purge wolfram-engine
sudo apt-get remove --purge libreoffice*
sudo apt-get clean
sudo apt-get autoremove

  7. Installing and Configuring Pi-Hole

pi-Hole dashboard
1. SSH into your RPi and type:

curl -sSL https://install.pi-hole.net | bash

Walk through the text based wizard and accept all of the default values. When it asks you for which DNS server to use, select one that you feel most comfortable with. Later in this blog post we will install and configure DNSCrypt, so doesn't really matter what you select now. Make sure at the end you write down the admin console password at the very end of the installer wizard.

2. There are a lot of block lists out there, but here are a few that should get you around 1.7M blocked domains. Login to Pi-Hole (http://YourIP/admin), click on Settings, then blocklists. Paste all at once the list below and click on 'Save and Update'. For a good discussion on more block lists you can check out the PI-Hole forum here.
**wait and do this later in webinterface from pi hole if you need it more black/white list stuff**

https://blocklist.site/app/dl/malware
https://blocklist.site/app/dl/ransomware
https://blocklist.site/app/dl/tracking
https://blocklist.site/app/dl/fraud
https://blocklist.site/app/dl/phishing
https://v.firebog.net/hosts/AdguardDNS.txt
https://1hos.cf
https://zeustracker.abuse.ch/blocklist.php?download=domainblocklist
https://hosts-file.net/grm.txt
https://reddestdream.github.io/Projects/MinimalHosts/etc/MinimalHostsBlocker/minimalhosts
https://raw.githubusercontent.com/StevenBlack/hosts/master/data/KADhosts/hosts
https://raw.githubusercontent.com/StevenBlack/hosts/master/data/add.Spam/hosts
https://v.firebog.net/hosts/static/w3kbl.txt
https://v.firebog.net/hosts/BillStearns.txt
https://adaway.org/hosts.txt
https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt
https://v.firebog.net/hosts/Easyprivacy.txt
https://raw.githubusercontent.com/quidsup/notrack/master/trackers.txt
https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt
https://raw.githubusercontent.com/Perflyst/PiHoleBlocklist/master/SmartTV.txt
https://s3.amazonaws.com/lists.disconnect.me/simple_malvertising.txt
https://www.malwaredomainlist.com/hostslist/hosts.txt
https://dbl.oisd.nl/

3. With all those blocked domains, there are a few we want whitelisted to prevent possible web surfing issues. I found this list below, but feel free not to use it or find your own whitelist. Or don't whitelist anything until you run into an issue and then try to resolve it on the spot.

git clone https://github.com/anudeepND/whitelist.git
cd whitelist/scripts
sudo chmod +x whitelist.sh
sudo ./whitelist.sh

4. After some web surfing, I found that Facebook comments were somewhat broken with all the blocks. So I had to add the following two whitelist entries for comments to fully work:

b-graph.facebook.com
graph.facebook.com

5. If you used all of the block lists above, be prepared to troubleshoot apps or websites that don't work because of blocked domains. If you run across a non-functional site or app, review the Pi-Hole logs for blocked domains and try whitelisting one at a time and re-testing your site/app to see what fixes the problem.

8. Installing and Configuring DNSCrypt

Note: If you are using the Eero Wi-Fi mesh system, see the bottom of this blog post about DNSCrypt and Eero secure. If you are using Eero secure you won't need DNSCrypt. If you aren't using Eero secure, go ahead and install DNSCrypt. Also, if you want to use NextDNS.io for an additonal layer of DNS security and filtering (highly recommended), go through this section, then follow the instructions later in this section.

1. Enter the following commands to do a base DNSCrypt installation. As this blog post ages, the exact download path WILL change. Check out the latest version listing here and modify the wget and tar commands as needed to use the latest binary.

cd /opt

sudo wget https://github.com/jedisct1/dnscrypt-proxy/releases/download/2.0.31/dnscrypt-proxy-linux_arm-2.0.31.tar.gz

sudo tar -xf dnscrypt-proxy-linux_arm-2.0.31.tar.gz
sudo mv linux-arm dnscrypt-proxy && cd dnscrypt-proxy
sudo cp example-dnscrypt-proxy.toml dnscrypt-proxy.toml
sudo nano dnscrypt-proxy.toml

Under Global settings add one or more servers. I like the iOS app DNSCloak to sift through the plethora of servers you can use. For example, you can search for DNS servers that block ads, support doh (DNS over HTTPS), view locations, etc. I like AdGuard DOH. You can also check out the public DNSCrypt server list here and pick one or more that fits your requirements. Note: If you want to use DNSNext.io service, just leave the default servers here and we will configure this later in this section.
server_names = ['adguard-dns']

Under List of Local addresses change the port number to something you like, above 1024. I'm using 5350 in this example. Pi-Hole will be using port 53 (standard for DNS), so that's why we must use a custom port number for DNSCrypt.

listen_addresses = ['127.0.0.1:5350', '[::1]:5350']
Change the following:

require_dnssec = true
require_nofilter = false
cache = false     (we will use the Pi-Hole cache)

Anyonmized DNS is a new feature in the most recent versions of DNSCrypt. If you want to use this feature, scroll all the way down to the bottom of the TOML file. For server_name add the same server name you used above. For the 'via' servers, review the relay list here, and pick a couple that suite your needs. I used servers in close proximity to my house. You may want to use servers in a different country, or have other unique requirements. Note: If you want to use NextDNS.io service, you don't want to anonymize your queries, so don't configure this section.

routes = [
    { server_name='adguard-dns', via=['anon-cs-usca', 'anon-cs-ca2'] },
 ]

Save and exit the nano editor.

**3. Installing and Configuring DNSCrypt**

Note: If you are using the Eero Wi-Fi mesh system, see the bottom of this blog post about DNSCrypt and Eero secure. If you are using Eero secure you won't need DNSCrypt. If you aren't using Eero secure, go ahead and install DNSCrypt. Also, if you want to use NextDNS.io for an additonal layer of DNS security and filtering (highly recommended), go through this section, then follow the instructions later in this section.

1. Enter the following commands to do a base DNSCrypt installation. As this blog post ages, the exact download path WILL change. Check out the latest version listing here and modify the wget and tar commands as needed to use the latest binary.

cd /opt

sudo wget https://github.com/jedisct1/dnscrypt-proxy/releases/download/2.0.31/dnscrypt-proxy-linux_arm-2.0.31.tar.gz

sudo tar -xf dnscrypt-proxy-linux_arm-2.0.31.tar.gz
sudo mv linux-arm dnscrypt-proxy && cd dnscrypt-proxy
sudo cp example-dnscrypt-proxy.toml dnscrypt-proxy.toml
sudo nano dnscrypt-proxy.toml

Under Global settings add one or more servers. I like the iOS app DNSCloak to sift through the plethora of servers you can use. For example, you can search for DNS servers that block ads, support doh (DNS over HTTPS), view locations, etc. I like AdGuard DOH. You can also check out the public DNSCrypt server list here and pick one or more that fits your requirements. Note: If you want to use DNSNext.io service, just leave the default servers here and we will configure this later in this section.
server_names = ['adguard-dns']

Under List of Local addresses change the port number to something you like, above 1024. I'm using 5350 in this example. Pi-Hole will be using port 53 (standard for DNS), so that's why we must use a custom port number for DNSCrypt.

listen_addresses = ['127.0.0.1:5350', '[::1]:5350']
Change the following:

require_dnssec = true
require_nofilter = false
cache = false     (we will use the Pi-Hole cache)

Anyonmized DNS is a new feature in the most recent versions of DNSCrypt. If you want to use this feature, scroll all the way down to the bottom of the TOML file. For server_name add the same server name you used above. For the 'via' servers, review the relay list here, and pick a couple that suite your needs. I used servers in close proximity to my house. You may want to use servers in a different country, or have other unique requirements. Note: If you want to use NextDNS.io service, you don't want to anonymize your queries, so don't configure this section.

routes = [
    { server_name='adguard-dns', via=['anon-cs-usca', 'anon-cs-ca2'] },
 ]

Save and exit the nano editor.

**4. Testing DNSCrypt**

1. Now we need to start the service and test it to make sure it's working before we configure Pi-Hole to use it.

sudo ./dnscrypt-proxy -service install
sudo ./dnscrypt-proxy (validate the server runs without errors, then control-C to stop)
sudo ./dnscrypt-proxy -service start
sudo systemctl status dnscrypt-proxy

The sudo ./dnscrypt-proxy command will provide detailed startup information and return any errors it encounters.  sudo systemctl status dnscrypt-proxy does the same for DNScrypt when it's started as a service. Both should have the same output, as shown below. If the Anonymized setting is properly configured those relay servers will be shown in the DNSCrypt output.

DNSCrypt Anonymized routing
To run a quick test that DNSCrypt can perform name resolution type:

./dnscrypt-proxy -resolve www.aol.com


**5. Configuring Pi-Hole for DNSCrypt**

1. Login to the Pi-Hole console (http://RPIAddress/admin). Go to Settings, DNS. Uncheck all upstream DNS servers and enter 127.0.01#5350 in Custom 1 (IPv4) and tick the box. For IPv6, enter ::1#5350 If you are running a VPN server on your Raspberry Pi, you will likely need to change the listening behavior to listen on all interfaces. Save the change.

Pi-Hole DNS Configuration
2. If you configured the Raspberry Pi for a static IP address, you can change the DNS server to point to localhost so that all Raspberry Pi DNS requests get filtered and encrypted. Find the eth0 section and change the DNS server to 127.0.0.1.

sudo nano /etc/dhcpcd.conf

3. Reboot your Raspberry Pi and verify all DNS queries are working properly and that Pi-Hole is blocking some requests.

sudo reboot
ssh pi@RPiAddress
nslookup www.aol.com (should return real addresses)
nslookup www.aol.com RPiAddress (e.g. nslookup www.aol.com 10.13.2.200)
nslookup xp.apple.com (should return 0.0.0.0 as it's blocked)

Both AOL results should return valid public IP addresses. The XP.apple.com address should NOT work and just give you 0.0.0.0 since it is on the block list. If the apple address returns a valid public IP, something is misconfigured.

9. Reconfiguring your Router for Pi-Hole

The final step is to reconfigure your network to use the new Pi-Hole DNS server. First, any devices with a static IP address need to have their DNS changed to only use the RPi address. Second, if your network uses DHCP (most home networks do), the DHCP/router will need to be reconfigured to use the new Pi-Hole DNS server. The exact steps vary widely, so I can't cover all options here. I use the Eero mesh Wi-Fi system, so it's a simple matter of opening the Eero app then going to Network Settings, Advanced settings, DNS, Custom DNS, and enter the Pi-Hole IP address. The mesh will then reboot. After your router reboots, open a command prompt or shell on your computers and do various lookups:

nslookup www.aol.com (should return real addresses)
nslookup xp.apple.com (should return 0.0.0.0 as it's blocked)

If permitted lookups work and the blocks work as well, then your device is fully utilizing Pi-Hole and DNSCrypt! Congratulations.


6. If DNS cannot be set to the pi hole ip (same as the one to connect to webinterface) on your ROUTER then every device needs to change DNS manually

iOS: wifi settings of current ssid dns set to manual and delete all entries. Add pi hole ip e.g. for me: 192.168.0.17 (it is the static ip you have chosen for pi hole not its 127.0.0.1 DNS!)

Note: PI hole or custom DNS only works on wifi for cellular you gotta use 3rd party app that creates an empty vpn just to change the dns on the go. GOOOGLE IT!!!!

LINUX: #IP FROM PI HOLE

$ sudo nano /etc/dhcpcd.conf
Add at the end of file:
static domain_name_servers=194.168.0.17

$ sudo service dhcpcd restart

Windows: network settings adapter properties ip4 properties and deactivate ip6
