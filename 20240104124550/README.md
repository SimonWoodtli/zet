# Server Security: Additional SSH Hardening

> 🧐 restart systemd sshd service often to apply settings and check if they
> work or error out.

## Changing the default SSH port

Some people like to change the default 22 SSH port to a different port. However
this does not add any additional security as any script kiddie can still scan
for open ports using `nmap`. The only thing you gain is fewer log entries to
deal with, because many malicious programs that scan your server run on port
22. That's all you get fewer login attempts ergo fewer entries in your logs.

If you still decide to configure ssh with a custom port it's paramount to use a
low privileged port (< 1024) or  add SSHFP. Although these ports are often
reserved for programs, their services do require root to run.

> 🧐 If you change the default port in the sshd config you also need to update
> the firewall `ufw allow xx/tcp` and deny the default 22 port `ufw deny
> 22/tcp`

***Here is why***:
Let's assume we configure ssh with port 2222 and let's assume that I am an
attacker with initial access. I am aware of an exploit that shuts down the SSH
daemon, causing a denial-of-service (DoS) situation. Since the port is a
"non-privileged" port, I can load tools like `netcat` or any other malicious
software to listen on port 2222 because the system allows me to open that port
in the user context. In this scenario, I can spin up a fake SSH server to phish
your credentials.

Yes, you will likely see a host-key warning, but let's be honest it's not
uncommon for users to ignore or overlook such warnings. If you decide to change
the port to a value above the "well-known" ports, it is advisable to implement
other security measures like SSHFP.

### Best Practice: Deny all incoming ports on your server

Best practice is to not have any open ports on your server (unless a service
that you run requires that ofc). Then there are two ways how to actually
establish a ssh connection from your client.

1. V1: Only allow dedicated/very specific IPs to make connections to your server.
   (Firewall way)
1. V2: Setup a Wireguard VPN on the server, allow only LAN connections on the
   server. Which in practice means which the client needs to be connected to
   the VPN tunnel in order to connect to the server. Downside it gets a bit
   complicated to actually host any publicly available services, but if you
   don't have any that's cool. Otherwise V3 is great.
1. V3 (V1+V2): Create a Wireguard VPN on a separate, dedicated remote VPS with
   openBSD. And allow all your web servers or servers you manage to only accept
   SSH connection from that VPN/IP.

Both these methods eliminate the use of fail2ban since we created an almost
ghost server (V1) or a ghost server (V2). In other words no more port scanning,
brute forcing are happening since the server is not targetable. However as a
backup security measurement it still doesn't hurt to use it.

1. Best practice V1: Use firewall to only allow ssh connections from specific
   IPs. (good if your client has a static public IP, otherwise annoying to keep
   updating the firewall rule, or create a cronjob with a bash script to update
   the rule automatically :o)
1. Best practive V2: The tunnel VPN way (easy mode: just install Tailscale), or
   use a self hosted access proxy like [Teleport] with 2FA

## Additional /etc/ssh/sshd_config settings:

These settings can be found on [Mozillas infosec site][infosec].

```
# Supported HostKey algorithms by order of preference.
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key

KexAlgorithms curve25519-sha256@libssh.org,ecdh-sha2-nistp521,ecdh-sha2-nistp384,ecdh-sha2-nistp256,diffie-hellman-group-exchange-sha256

Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr

MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,umac-128@openssh.com

# LogLevel VERBOSE logs user's key fingerprint on login. Needed to have a clear audit track of which key was using to log in.
LogLevel VERBOSE
```

### Client Side hardening: /etc/ssh/ssh_config

> 🧐 This configuration is less compatible and you may not be able to connect
> to some servers which use insecure, deprecated algorithms. Nevertheless,
> modern servers will work just fine.

```
# Ensure KnownHosts are unreadable if leaked - it is otherwise easier to know which hosts your keys have access to.
HashKnownHosts yes
# Host keys the client accepts - order here is honored by OpenSSH
HostKeyAlgorithms ssh-ed25519-cert-v01@openssh.com,ssh-rsa-cert-v01@openssh.com,ssh-ed25519,ssh-rsa,ecdsa-sha2-nistp521-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp256-cert-v01@openssh.com,ecdsa-sha2-nistp521,ecdsa-sha2-nistp384,ecdsa-sha2-nistp256

KexAlgorithms curve25519-sha256@libssh.org,ecdh-sha2-nistp521,ecdh-sha2-nistp384,ecdh-sha2-nistp256,diffie-hellman-group-exchange-sha256
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256,umac-128@openssh.com
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
```

## Wait there is more: Further things to consider

1. Create your ssh key pair with yubikey for additional security
1. Create your ssh key pair with a passphrase for additional security
1. If it's a homelab LAN server consider setting up a static IP for your
   server: `vi /etc/netplan/01-netcfg.yaml` and set `dhcp no` and the
   `address: ...` (choose IP outside of DHCP scope) and apply it `netplan
   apply`. Then on your router make sure the IP is outside of the DHCP scope.
1. TODO: research systemd port knocking

[Teleport]: <https://goteleport.com/>
[infosec]: <https://infosec.mozilla.org/guidelines/openssh.html>

Related:

* <https://infosec.mozilla.org/guidelines/openssh>
* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104134254](/20240104134254/) Server Security and Setup: Make it cosy
* [20240104124550](/20240104124550/) Server Security: Additional SSH Hardening
* [20240104130222](/20240104130222/) Server Security: Config ufw Firewall
* [20240201232741](/20240201232741/) Common methods to expose local Homelab services to the public

Tags:

      #linux #server #ssh #security #networking #firewall #infosec
