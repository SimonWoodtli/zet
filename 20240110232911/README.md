# LAN Server Install: Setup and Install dietpi

Currently I host some of my favourite LAN services and apps on a dietpi rpi4.
It's simple to get it up and running in no time without a lot of manual
intervention.

* TODO: On my next install check if dietpi user can be directly used after flash and use that user instead if possible.

## First steps

> 🧐 Consider using remote.it as a service for SBCs of friends and family in
> case you need to have easy remote ssh access to them without setting up a
> VPN.

1. Download dietpi and use rpi imager or balena etcher to flash it.
1. Start it up and on your dev machine look for ip: `nmap -sn yourDevIP/24` or just `ping dietpi`
1. Login with `root@dietpi` and pw `dietpi`
1. Go through installer
      1. Set Static IP outside of DHCP scope of router. On your router reserve that IP for dietpi.
      1. Change ssh to openSSH
      1. Change Timezone
1. Select software to be installed:
    1. `virtualHere syncthing homer dietpi-dashboard wireguard nginx certbot vim git`
1. From your dev machine: `ssh-copy-id -i ~/.ssh/yourkey.pub dietpi@dietpi`
1. Login with `ssh dietpi@dietpi`
1. Setup my dotfiles: [20240104134254](/20240104134254/) Server Security and Setup: Make it cosy
1. Setup sudo without pw prompt: `%sudo ALL=(ALL) NOPASSWD:ALL`
1. Reboot

> 📝 When I create a custom router either use `bind9` on the router.
> Or with OpenWRT you may directly overwrite the hosts you want. And some
> routers have an interface to write custom host entries.


# Why would you want to run your own DNS Server?

Now for privacy reasons and for convenience it makes sense to run your own DNS
server. One software that allows you to do so is `bind9`. There are different
ways to approach this:

1. On a custom router
    1. With OpenWRT or pdfSense you can do DNS host overwrites directly (this will give you local subdomains for your LAN server, but not more privacy)
    1. If you run other software on top of your router. You might want to add `bind9` or another DNS server on it. This will give you privacy whilst using the internet at home or connected to it via VPN tunnel.
1. On a local LAN sever with `bind9`: Like mentioned before you get privacy and custom domains for your LAN as long as you are connected to your network.
1. On a VPS sever with `bind9`: Now you loose a little privacy and it might be a bit slower than a DNS on your LAN. But you can use it on any network conveniently.


## Privacy explained - Why should I care?

## Convenience explained - Custom domain names why do I need that?

In short if you are running a local homelab and have some webapps and services
that have a web interface. You might want to access those sites more
conveniently.

In long:

Running your own DNS server allows you to add any custom domain names and
subdomains that you want to assign to any of your homelab servers. In other
words your webapps/services can now be accessed with e.g `https://sync.home`
instead of `http://192.168.0.5:8384`.  However for this to work you also need a
proxy site since these services don't run on port 80|443.

Why do we need both DNS and proxy? Because the domain name can only point to an
IP address. Since those webapps and services run on their own port we need a
proxy site too. That requires apache or nginx. At this point you can also
easily add certbot and pw authentication.

But not only homlab server would benefit from this also any remote server you
want to give a domain name would at least for you personally be reachable
without typing any pesky IPs or ports.


## Setup Wireguard

> 🧐 If you don't have a static IP, which is probably the case your public IP
> will change every once in a while (6 months or so). And if you do have a
> dynamic public IP it's best to have a cronjob on a machine at your LAN. Which
> runs a script to check the if the public IP has changed and if so send a
> notification. It'd be great to automatically update the A record of your
> domainname to point to the new IP, need to research if that is possible.
> An easier way would be to use no-ip or dynDNS dynamic DNS service.

1. Check if you are stuck behind CGNAT: `tracepath $(myip)` (turn your VPN off first). More than one hop means you are.
    1. If you are lucky and don't have CGNAT: During initial setup process enter your public IP.
    1. Also for convenience you might want to add your public IP to your domain registrar account, or use free duckDNS.
    1. If you have CGNAT: You need to setup A wireguard on a VPS (because of the stable IP) and use that as an entry point to than hop on to your home network.

TODO: Finish this when I have time for it because I do have CGNAT :(

## Setup syncthing


1. Test if syncthing is up: `systemctl status syncthing`. In browser go to http://dietpi:8384
1. Add 'sync.dietpi' as a subdomain using bind9:
1. Add syncthing as a nginx proxy with nginx password authentication: TODO add zet-links to both zets
1. Add SSL cert with for https: `certbot --nginx`

* <https://stackoverflow.com/questions/45967872/how-to-setup-local-domain-in-local-network-that-everyone-can-see>

## Setup Homer

TODO: finish

1. Add SSL cert with for https: `certbot --nginx`
* <http://dietpi/homer/>
* <https://dietpi.com/docs/software/system_stats/#homer>

## Setup Dietpi-Dashboard

TODO: finish

1. Add SSL cert with for https: `certbot --nginx`
* <http://dietpi:5252/>
* <https://dietpi.com/docs/software/system_stats/#dietpi-dashboard>

## Setup virtualHere

* <https://dietpi.com/docs/software/remote_desktop/#virtualhere>


* TODO: Add wireguard VPN or tailscale VPN setup

## Setup nginx

I use nginx for pw authentication of web interfaces and adding SSL certs with certbot.
TEST: Using an ngnix proxy should allow me to create a subdomain instead of using those pesky ports.





Related:

* <https://mikenet.uk/homelab/wireguard/networking/2022/05/03/routing-a-public-ip-over-wireguard-to-overcome-cgnat.html>
* <https://www.noip.com/>
* <https://account.dyn.com/>
* <https://dietpi.com/docs/getting_started/>
* <https://dietpi.com/docs/software/>
