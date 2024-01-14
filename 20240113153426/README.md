# Setup Service: DuckDNS with Auto SSL certs using reverse proxy and DNS validation for certs(DNS-01) for your homelab

Benefits:

* No custom DNS server required to get subdomains we use the CNAME record with
  wildcards from our public domain name provider
    * Works without any further per device configurations like manual DNS
      settings.
* No fiddling with any hostfiles as we just use a domain name provider to point
  to an internal IP
* No need to deal with certbot for every site/service you run for SSL, we just
  use let's encrypt DNS challenge (DNS-01)
* No need to remember all the IPs and ports as we got a subdomain and domain
  name with using a reverse proxy to point to your services
* Works even if you do not plan to host your services public but keep them on
  your LAN.

## Prerequisites: First Steps

1. Option 1: Login to https://www.duckdns.org/ and create a new domain name.
   Point it to the local IP of your homelab server.
1. Option 2: Login to your favourite DNS provider and point a A record to your
   local IP of your homlab server. Also add a CNAME record with a wildcard for
   subdomains.
1. Install docker: Via my install script or `sudo apt install docker.io` (on dietpi use `dietpi-software` instead)
1. Install docker-compose: `sudo apt install docker-compose-v2` (on dietpi use `dietpi-software` instead)
1. Choose between Nginx Proxy Manager, Caddy or Traefik as your reverse proxy and your auto SSL renewal
1. On your Router you might need to add an exception to 'DNS Rebinding Attack
   Protection' with the domain name you set so you can use public domain names
   that point to a local IP.

### Nginx Proxy Manager

FIXME: on dietpi after i setup my dotfiles I could no longer use the dietpi acc to `dietpi-software`. So I used root to install docker, but now means only root can run docker tried to add docker group to dietpi (didn't work). Redo the whole dietpi setup without my dotfiles. Then check if I cannot install software with `dietpi-software` this would be much better. I really don't wanna run docker as root.

> 🧐 Try to keep `dietpi-software` apps as minimal as possible and run
> everything via docker (easier to upgrade and manage containers)

TODO: research if they have docker img and add them too : virtualHere syncthing homer dietpi-dashboard portainer

1. Add config to ~/docker-compose.yml: (example)

TODO: add this config to my Private/ git repo

```
version: '2.2'
services:
  nginxproxymanager:
    image: 'jc21/nginx-proxy-manager:latest'
    container_name: nginxproxymanager
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./nginx/data:/data
      - ./nginx/letsencrypt:/etc/letsencrypt

  nextcloud:
    image: lscr.io/linuxserver/nextcloud:latest
    container_name: nextcloud
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Berlin
    volumes:
      - ./nextcloud/appdata:/config
      - ./nextcloud/data:/data
    restart: unless-stopped

  jellyfin:
    image: lscr.io/linuxserver/jellyfin:latest
    container_name: jellyfin
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Berlin
    volumes:
      - ./jellyfin/config:/config
      - ./jellyfin/tvshows:/data/tvshows
      - ./jellyfin/movies:/data/movies
    restart: unless-stopped
```

1. run `docker compose up -d`

#### Nginx Proxy Manager Setup

> 🧐 Adding Hosts/Proxy Sites: If you add a proxy site of a service that is not
> in the docker-compose file, but runs on the same device use the local IP of that device.
> If it is on another machine it's the local IP of that machine.
> Finally if it is in fact within the docker-compose file you can refer to the
> name you gave the service in the docker-compose file.

1. Open browser and login to nginx proxy manager: `local-IP-of-device:81`
    1. user: admin@example.com pw: changeme
1. Go to SSL Certificates, and click "Add SSL Certificate".
    1. Domain name (2 entries): main domain and wildcard for subdomains e.g. `foo.duckdns.org` and `*.foo.duckdns.org`
    1. Enable DNS Challenge: Select DuckDNS (or your provider)
    1. Credentials: Get the DuckDNS API token and paste it
    1. Propagation Seconds: if it fails, increase value (default is 30s)
    1. Enable TOS and add Cert

> 🧐 When you add a proxy site: If a service ships with https e.g. nextcloud,
> use Scheme: https
> 🧐 When you add a proxy site: If a service has websockets, cache assets or
> block common exploit as a feature you can enable those settings too.

Add nginxproxymanager as the root domain:

1. Go to Hosts, and click "Add Proxy Host"
1. Domain Names: foo.duckdns.org
1. Scheme: http
1. Forward Hostname / IP: nginxproxymanager
1. Forward Port: 81
1. Go to SSL tab and enable: Force SSL and HTTP/2

Add jellyfin:

1. Go to Hosts, and click "Add Proxy Host"
1. Domain Names: jellyfin.foo.duckdns.org
1. Scheme: http
1. Forward Hostname / IP: jellyfin
1. Forward Port: 8096
1. Go to SSL tab and enable: Force SSL and HTTP/2

Add syncthing (not in docker-compose, but same machine)

1. Go to Hosts, and click "Add Proxy Host"
1. Domain Names: sync.foo.duckdns.org
1. Scheme: http
1. Forward Hostname / IP: LOCAL-IP-OF-MACHINE (If syncthing was running on another device you'd use that IP)
1. Forward Port: 8384
1. Go to SSL tab and enable: Force SSL and HTTP/2

### Caddy

https://caddy.community/t/how-to-guide-caddy-v2-cloudflare-dns-01-via-docker/8007
https://samjmck.com/en/blog/using-caddy-with-cloudflare/
https://ssine.ink/en/posts/caddy-non-443-port-https/
https://caddyserver.com/features

### Traefik

TODO: write this config
https://yewtu.be/watch?v=liV3c9m_OX8


Related:

* <https://notthebe.ee/blog/easy-ssl-in-homelab-dns01/>
* <https://docs.linuxserver.io/general/docker-compose/>
