# Publish a Homelab service to the internet with SSH and reverse proxy via VPS

The idea is to remote port forward a running Homelab service to a VPS to make
it available on the VPS via the SSH tunnel. Then from the VPS we create a
reverse proxy with nginx to publish it to the internet.

Since the established tunnel is using SSH all traffic is encrypted which is
great. This method can be used either for temporarily sharing things with the
world or more permanent setups. IMO it's a great way to publish stuff to the
public.

## Prerequisites

1. Have a VPS up and running and hardened
1. Have a domain name registered and linked to the VPS
1. Have SSH keypair configured with your Homelab server(client) and the VPS
   (server)
1. Have nginx installed on your VPS and port 80/443 opened in firewall
1. Have certbot installed on your VPS with nginx module to create SSL certs

## First Steps

If you want the remote port forwarded service to be accessible by anyone use
'GatewayPorts yes'. If you want more granular control and only allow certain
IPs to access the service 'GatewayPorts clientspecified' (E.g. allows only
52... IP to connect to service: `ssh -R 52.194.1.73:8080:localhost:80
user@VPS_IP`)

1. Login to your VPS
1. Edit sshd: `sudo vi /etc/ssh/sshd_config`. Pick either 'yes' or
   'clientspecified' depending on your needs.

```
GatewayPorts yes
#GatewayPorts clientspecified
```

## Homelab Setup

> 🧐 `ssh -R ... user@IP`: allows binding only to registered port range.
> `ssh -R ... root@IP` allows all ports, registered & privileged

1. Run a service e.g something on port 2000 on your loopback interface
1. Publish the service via SSH remote port forward `ssh -N -R
   2000:127.0.0.1:2000`. This will bind the port 2000 on the VPS to this
   service.

> 🧐 If you need it to be a more permanent thing, use a systemd user service.

## VPS Setup

1. Check if service is incoming: `ss -tulpn` look for port 2000
1. Create a reverse proxy for localhost:2000 with nginx
1. Create an SSL cert: `certbot --nginx` for it

> 🧐 If you plan to make the service available permanently I strongly advice to
> add 2FA to make it more secure. (Authelia or OAuth)

Related:

* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104154437](/20240104154437/) Server Service: Add nginx
* [20240104162118](/20240104162118/) Add https cert for nginx
* [20240201232741](/20240201232741/) Common methods to expose local Homelab services to the public
* [20240108191653](/20240108191653/) Server Service: Add yt-local with nginx proxy and pw auth

Tags:

      #linux #homelab #server #VPS #SSH #service #tunnel #publish
