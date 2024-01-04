# Server Security: Config ufw Firewall

> ⚠️ If you plan to host docker apps it's better to use `firewalld` as docker
> iptables take priority and therefor cannot be configured with `ufw`. Or you
> can also just directly use iptables. (see. TODO: Zettel Link to Add Docker)

> 🧐 To check status: `ufw status verbose` and enable it if it's not already
> `ufw enable`

## Best Practice

1. allow all outgoing connections: `ufw default allow outgoing`
1. deny all incoming connections: `ufw default deny incoming`

Remember if you don't plan on using a VPN connection or only allow white listed
IPs you still need to open the SSH port: `ufw allow ssh`

## Docker ufw issue

TODO: Test which works: Then Create Zettel for these methods (with docker in mind), without docker `ufw` is a lot easier to setup.
* Link to Configure firewalld
* Link to Configure iptables

Related:

* TODO add zet serverinit, serverconfig, ssh hardening

Tags:

    #linux #server #security #firewall #networking #infosec
