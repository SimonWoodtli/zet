# Server Service: Add syncthing

> 🧐 I prefer to use syncthing on  a RPI4 or homelab server on my LAN. The
> reason being I want to be able to sync more sensitive data with it. And you
> should never trust a public server you don't own with any personal data
> including keys,pws etc. for you don't have control over it. Encryption
> standards evolve so does the ability to crack them (see quantum computing).
> Better safe than sorry :O

Now you can totally use rsync to sync files between computers. And I often do,
but some files need to be synced to my phone too. And whilst you can connect to
it with Termux. It actually requires some manual steps and I haven't found a way
to automate that process. For use cases like that syncthing is great.


## Install and Setup LAN only sync:

I personally prefer to use syncthing only on my LAN.

### Debian Server

TODO: Test this out and adjust notes after that

1. Install it: `apt install syncthing`
1. Open ports: `ufw allow 22000; ufw allow 21027/UDP; ufw allow 8384; ufw allow 22010/tcp; ufw allow 22011/tcp; ufw allow 22012/tcp`
1. Enable&Start systemd service:
    1. copy service to your users config: `sudo cp ... ~/.config/systemd/user/`
    1. `systemctl enable syncthing@myuser.service`
1. Setup nginx proxy for syncthing web interface (8384 is web interface port)
1. Add nginx password protection to the site
1. Enable certbot https for the proxy site: `certbot --nginx`

Next up:
* Configure syncthing form web interface

### RPI4

TODO: write an ansible playbook to automate all the services/apps I use on pi4.

1. Install headless DietPi
2. 


## Install and Setup remote Server:

Basically the same as with a local Debian install. But if you use docker
remember don't use `ufw` with it.

## Configure syncthing from web interface:

Related:

* <https://docs.syncthing.net/users/firewall.html>
* <https://wiki.archlinux.org/title/Syncthing>
* <https://docs.syncthing.net/users/autostart.html#linux>
