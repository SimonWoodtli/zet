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

## Install syncthing

### Install via docker

I like the [linuxserver.io image][linuxserver] so just use their docker-compose.yml and `docker compose up -d` to start it.

* change the hostname to the one of the machine you run it on (optional)
* change the volumes: the config/ is appdata that you need to have a persistent configuration of syncthing
* change the volumes: for each folder you want to share you need to create a entry /my/host/dir/location:docker/location

### Install via podman

I only tried the official docker.io syncthing image, gotta test linuxservers
image some time.

For podman you currently have to run as root so `sudo podman compose up -d`.
And you also need 'privileged: true' in order to get it to work.

I also like to use 'network_mode: host' and the env var. 'STGUIADDRESS=' to
make it run on 127.0.0.1 instead of the default 0.0.0.0 (depending on the
machine).

* change the hostname (see docker install)
* change the volumes same (see docker install)

### Install via package manager (Debian/VPS)

1. Install: `apt install syncthing`
1. Open ports: `ufw allow 22000; ufw allow 21027/UDP; ufw allow 8384`
1. Disable the systemd system service and create a user service instead [see docs][docs]
1. Setup nginx proxy for syncthing web interface (8384 is web interface port)
1. Add nginx password protection to the site
1. Enable certbot https for the proxy site: `certbot --nginx`

## Next up: Config Syncthing from web UI

* [20240124232458](/20240124232458/) Syncthing configuration and caveats

[linuxserver]:<https://docs.linuxserver.io/images/docker-syncthing/>
[docs]:<https://docs.syncthing.net/users/autostart.html#linux>

Related:

* <https://docs.syncthing.net/users/firewall.html>
* <https://wiki.archlinux.org/title/Syncthing>
* [20240112144725](/20240112144725/) Why would you want to run your own DNS Server?
* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240110232911](/20240110232911/) LAN Server Install: Setup and Install dietpi
* [20240113153426](/20240113153426/) Setup Service: DuckDNS with Auto SSL certs using reverse proxy and DNS validation for certs(DNS-01) for your homelab

Tags:

    #linux #server #syncing #files #rsync #service
