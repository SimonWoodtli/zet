# Server Security: Config ufw Firewall

> ⚠️ If you plan to host docker apps it's better to use `firewalld` as docker
> iptables take priority and therefor cannot be configured with `ufw`. Or you
> can also just directly use iptables. (see. TODO: Zettel Link to Add Docker)

> 🧐 To check status: `ufw status verbose` and enable it if it's not already
> `ufw enable`

## Best Practice

1. Allow all outgoing connections: `ufw default allow outgoing`
1. Deny all incoming connections: `ufw default deny incoming`

Remember if you don't plan on using a VPN connection or only allow white listed
IPs you still need to open the SSH port: `ufw allow ssh`

## Docker ufw issue

TODO: Test which works: Then Create Zettel for these methods (with docker in mind), without docker `ufw` is a lot easier to setup.
* TODO zet - Link to Configure firewalld
* TODO zet - Link to Configure iptables

Related:

* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104134254](/20240104134254/) Server Security and Setup: Make it cosy
* [20240104124550](/20240104124550/) Server Security: Additional SSH Hardening
* [20240104010508](/20240104010508/) Server Security: Add fail2ban
* [20240104012938](/20240104012938/) Server Service: Add and Secure Docker

Tags:

    #linux #server #security #firewall #networking #infosec
