# LAN Server Install: Setup and Install dietpi

Currently I host some of my favourite LAN services and apps on a dietpi rpi4.
It's simple to get it up and running in no time without a lot of manual
intervention. And it is minimal so not too many resources get wasted.

However normally I'd prefer Ubuntu server VMs on a proxmox host for a homelab.
But I need some time to get that going again on my server. And for a quick and
dirty backup dietpi is still great, it's just not very reusable out of the box.
 
## First steps: Install dietpi

> 🧐 Consider using remote.it as a service for SBCs of friends and family in
> case you need to have easy remote ssh access to them without setting up a
> VPN. Another great way is to use CloudFlare Tunnel or an open source [alternative].

### As root user

1. Download dietpi and use rpi imager or balena etcher to flash it.
1. Start it up and on your dev machine look for ip: `nmap -sn yourDevIP/24` or just `ping dietpi`
1. Login with `shh root@dietpi` and pw `dietpi` (let system update)
1. Go through installer
      1. Change ssh to openSSH
1. Select software to be installed: None! (keep a minimal install for root user)
1. Reboot

### As dietpi user

> ⚠️ Don't use my dotfiles repo/config, for some reason after doing that using
> `dietpi-...` commands stop working for dietpi user (I guess cause of bashrc)

1. Login with `ssh dietpi@dietpi`
1. Install software with `sudo dietpi-software` select:

```
virtualHere dietpi-dashboard vim git docker docker-compose portainer (wireguard)
```

3. From your dev machine: `ssh-copy-id -i ~/.ssh/yourkey.pub dietpi@dietpi`
3. Setup sudo without pw prompt: `%sudo ALL=(ALL) NOPASSWD:ALL`
3. `sudo dietpi-config`
   1. Performance Options: overclock arm high profile
   1. Language/Regional Options: Change Timezone
3. Reboot

## Next Steps: Give Services/Apps with web interfaces SSL certs and a custom subdomain name (and pw auth)

Add auto SSL certs and reverse proxy with public domain pointing to dietpis local IP:
* [20240113153426](/20240113153426/) Setup Service: DuckDNS with Auto SSL certs using reverse proxy and DNS validation for certs(DNS-01) for your homelab

Password Authentication:

If LAN: Add password authentication using caddy or nginx proxy manager:

If exposed public: Only add services that have 2FA integrated or add OAuth yourself together with 2FA via Authelia. TODO add zet here
      1. To actually expose them use Cloudflare Tunnel or tunnel.pyjam.as: TODO add zet here

> 🧐 If you don't share any services with other people it's probably safer and
> more convenient to not make anything public but instead use a VPN tunnel.

## Setup Portainer

Not sure yet if I should install portainer within the docker-compose file or
with `dietpi-software`. Currently runs with the latter.

* <https://codeopolis.com/posts/beginners-guide-to-portainer/>
* <https://earthly.dev/blog/portainer-for-docker-container-management/>

## Setup Wireguard

> 🧐 If you don't have a static IP, which is probably the case your public IP
> will change every once in a while (6 months or so). And if you do have a
> dynamic public IP it's best to have a cronjob on a machine at your LAN. Which
> runs a script to check the if the public IP has changed and if so send a
> notification. It'd be great to automatically update the A record of your
> domainname to point to the new IP. Which can be done with most domain name
> providers like namecheap with their API. Checkout [`ddclient`][ddclient]
> Alternatively you could use a dynamic DNS service like no-ip or dynDNS (not
> my cup of tea).

1. Install wireguard via `dietpi-software`
1. Check if you are stuck behind CGNAT: `tracepath $(myip)` (turn your VPN off
   first). More than one hop means you are.
    1. If you are lucky and don't have CGNAT: During initial setup process
       enter your public IP.
    1. Also for convenience you might want to add your public IP to your domain
       registrar account, or use free duckDNS.
    1. If you have CGNAT: You need to setup A wireguard on a VPS (because of
       the stable IP) and use that as an entry point to than hop on to your
       home network.

TODO: Finish this when I have time for it, because I currently am stuck behind CGNAT :(

If you are stuck behind CGNAT an easy alternative is to setup tailscale, headscale, netbird: TODO add zet here

## Test Services and configure them

1. Test if syncthing is up: `systemctl status syncthing`. In browser go to http://dietpi:8384
1. Test if your DuckDNS with the syncthing subdomain is working 'sync.myduckdns.duckdns.org' it should also have a SSL cert so https is up.
1. Configure syncthing and backup their config files somewhere so you don't have to keep doing this.

Now do the same for all the other services: homer, dietpi-dashboard, virtualHere (does that even have webUI?),

* TODO add zet: config sync zet here
* TODO add zet: config homer zet here
* TODO add zet: config virtualHere zet here
* TODO add zet: config dietpi-dashboard zet here

```
http://dietpi/homer/
https://dietpi.com/docs/software/system_stats/#homer
http://dietpi:5252/
https://dietpi.com/docs/software/system_stats/#dietpi-dashboard
https://dietpi.com/docs/software/remote_desktop/#virtualhere
```

[ddclient]:<https://github.com/ddclient/ddclient >
[alternative]: <https://github.com/anderspitman/awesome-tunneling>

Related:

* <https://mikenet.uk/homelab/wireguard/networking/2022/05/03/routing-a-public-ip-over-wireguard-to-overcome-cgnat.html>
* <https://www.noip.com/>
* <https://account.dyn.com/>
* <https://dietpi.com/docs/getting_started/>
* <https://dietpi.com/docs/software/>
* [20240112144725](/20240112144725/) Why would you want to run your own DNS Server?
* [20240113153426](/20240113153426/) Setup Service: DuckDNS with Auto SSL certs using reverse proxy and DNS validation for certs(DNS-01) for your homelab
* TODO add zet how to setup cloudflare tunnel

Note about cloudflare tunnel or similiar methods:
```
FYI, tunnels will not work if your power goes out or you restart your server.
You'll need to create a proper docker yml to restart always. Also, since
everyone has access to the domain names, you can restrict access in Cloudflare
with application policies which is highly recommended.
```
