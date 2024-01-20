# Fresh Server: First Steps

* TODO: create an ansible `serverinit` playbook

> 🧐 Don't add the SSH public key via your VPSs website. This would add the key
> for the root users home, which we'll disable anyways.

> ⚠️ As always with daemons remember to restart the systemd service to apply
> changes in the config files.

1. Start tmux
1. 1st window - login: Update system (keep that connection going, in case something goes wrong)
1. 2nd window - login: Configure stuff
1. Disable root bash history:

```
echo "HISTFILESIZE=0" >> ~/.bashrc
history -c; history -w
source ~/.bashrc
```

5. Change root pw set by VPS: `passwd root`
    1. To get a decent safe password: `openssl rand -base64 24` or use your password managers generator.
5. Add new user group: `sudo groupadd xnasero` (non existent groups can't be added with `useradd` directly)
5. Create regular user with sudo access (Fedora uses wheel, Debian uses sudo): `useradd -m -g xnasero -G users,sudo,adm -s /bin/bash -c admin-account xnasero`
    1. Change pw: `passwd xnasero`
    1. Turn off bash history for this user too: (Check earlier step for root)
5. Check if wheel|sudo group is activated/exists on your OS or add it manually: `vsudo` look for `%wheel  ALL=(ALL)       ALL` to double check: `sudo -l -U xnasero`
5. Disable ssh root login: `vi /etc/ssh/sshd_config` look for `PermitRootLogin yes` change to `no`
5. If you don't have an ssh key pair generate one on your local machine. Then we copy the public key to the server: `ssh-copy-id -i path/to/certificate xnasero@remote_host`
    1. Open new tmux window and try to connect with username and see if it works (no pw prompt, logged in as xnasero)
5. Disable ssh password login: `vi /etc/ssh/sshd_config` add `AuthenticationMethods publickey` (Mozilla recommends this config)
5. Disable root login altogether: `sudo passwd -l root` (still possible via `sudo su - root`)
5. Disable IPV6: Only do this if your VPS has IPV4 only and does not support IPV6

```
cp /etc/sysctl.conf /etc/sysctl.conf.backup
cat << "EOF" >> /etc/sysctl.conf
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
EOF
sysctl -p
```

## Next Steps

1. Config server: [20240104134254](/20240104134254/) Server Security and Setup: Make it cosy
    1. (I prefer) Configure `nftables` Firewall: TODO add zet
    1. (If not using nftables) Configure `ufw` Firewall: [20240104130222](/20240104130222/) Server Security: Config ufw Firewall
    1. More ssh hardening: [20240104124550](/20240104124550/) Server Security: Additional SSH Hardening
    1. Add fail2ban: [20240104010508](/20240104010508/) Server Security: Add fail2ban
1. Add domain name: [20240107205508](/20240107205508/) Server Setup: Add domain name to your Server
1. Add data backup system: This is different on each server and depends on the services and the data that you want to backup.
1. Add update system: TODO ADD ZEt
1. Add services

## Add Services

Add and config the services/apps you want to run on your server.

> 🧐 In general avoid exposing any admin interfaces to the public internet. Use
> a VPN tunnel or DMZ for it to access them.

> 🧐 Use app armor (`apparmor_status`) and docker images for additional
> security. As per app profiles and further isolation from the main OS can't
> hurt.

> 🧐 Avoid using any unsecured protocols. If you host some services/apps. E.g
> your running a webapp with authentication. Don't use port 80 since it's
> unsecured, that means this unencrypted traffic can be read by anyone. Instead use
> https port 443 with SSL cert. But many services/apps don't come with that out
> of the box. In that case you can use a reverse proxy. Client -> https/reverse
> proxy -> http/web app
> A reverse proxy is a web server that sits in front of your application and it
> forwards any client requests to them. For self hosted hobby projects nginx
> proxy manager is great, otherwise look into web application firewalls.

* Add Wireguard VPN: TODO add zettel
* Add Tailscale VPN: TODO add zettel
* [20240104154437](/20240104154437/) Server Service: Add nginx
* [20240104012938](/20240104012938/) Server Service: Add and Secure Docker
* [20240108132658](/20240108132658/) Server Service: Add email server
* [20240108191653](/20240108191653/) Server Service: Add yt-local YT frontend
* [20240109173457](/20240109173457/) Server Service: Add mumble server
* [20240107222637](/20240107222637/) Server Service: Add a data server with nginx
* [20240110174327](/20240110174327/) Server Service: Add syncthing

Related:

* <https://landchad.net/>

Tags:

        #linux #server #setup #sysadmin
