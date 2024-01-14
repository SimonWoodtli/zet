# Server Service: Add syncthing

> 🧐 I prefer to use syncthing on  a RPI4 or homelab server on my LAN. The
> reason being I want to be able to sync more sensitive data with it. And you
> should never trust a public server you don't own with any personal data
> including keys,pws etc. for you don't have control over it. Encryption
> standards evolve so does the ability to crack them (see quantum computing).
> Better safe than sorry :O

Now you can totally use rsync to sync files between computers. And I often do,
but some files need to be synced to my phone too. But syncthing makes things
just so much simpler.

## Homelab Dietpi or Ubuntu Server:

* [20240110232911](/20240110232911/) LAN Server Install: Setup and Install dietpi
* [20240113153426](/20240113153426/) Setup Service: DuckDNS with Auto SSL certs using reverse proxy and DNS validation for certs(DNS-01) for your homelab

## VPS Debian Server

1. Install it: `apt install syncthing`
1. Open ports: `ufw allow 22000; ufw allow 21027/UDP; ufw allow 8384; ufw allow 22010/tcp; ufw allow 22011/tcp; ufw allow 22012/tcp`
1. Enable&Start systemd service:
    1. copy service to your users config: `sudo cp ... ~/.config/systemd/user/`
    1. `systemctl enable syncthing@myuser.service`
1. Setup nginx proxy for syncthing web interface (8384 is web interface port)
1. Add nginx password protection to the site
1. Enable certbot https for the proxy site: `certbot --nginx`

## Next up: Config Syncthing from web UI

TODO add zet how to config here (make it LAN only sync if you don't use syncthing on a VPS)

Related:

* <https://docs.syncthing.net/users/firewall.html>
* <https://wiki.archlinux.org/title/Syncthing>
* <https://docs.syncthing.net/users/autostart.html#linux>
* [20240112144725](/20240112144725/) Why would you want to run your own DNS Server?
* [20240104161046](/20240104161046/) Fresh Server: First Steps
