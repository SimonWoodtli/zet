# Common methods to expose local Homelab services to the public

1. Any client with client VPN setup: Wireguard VPN Server on VPS, client VPN on
   Homelab server, client VPN on phone or any device outside
    * Security: Secure if VPS is hardened and up2date
    * Security: Only exposes Wiregurad Port, so pretty safe
    * ISP: works with static/dynamic IP or CG-Nat
    * 1 to 1 to 1 connection
1. Any client with client VPN setup: Wireguard VPN Server on Homelab,  client
   VPN on phone or any device outside
    * Security: Only exposes Wiregurad Port (router port forwarding required),
      so pretty safe
    * ISP: not possible with CG-NAT
    * ISP: works if static public IP
    * ISP: If dynamic public IP use DDNS. Or use Domain Name API and a custom
      script or `ddclient`
    * 1 to 1 connection
1. Any client with client VPN setup: Tailscale VPN on Homelab, Tailscale VPN on
   phone or any device outside
    * Security: Secure out of the box but trusting 3rd party (I'm sure it has
      some issues I am not aware of)
    * ISP: works with static/dynamic IP or CG-Nat
    * 1 to 1 to 1 connection? TODO research how it actually works
1. Any client with 0 setup: Remote port forwarding via SSH connection from
   Homlab to expose the service to a VPS. On VPS reverse proxy to expose to
   public
    * Security: Secure if VPS is hardened and up2date
    * Security: Service publicly exposed -> use 2FA for service
    * ISP: works with static/dynamic IP or CG-Nat
    * Simple setup - on local machine: `ssh -N -R 81:127.0.0.1:81 root@VPS_IP`
      and then create a reverse proxy on VPS. And add 'GatewayPorts yes' or
      better yet 'GatewayPorts clientspecified' to sshd config on VPS
    * Services use bandwidth from VPS when using them? Could get expensive (TODO research)
    * 1 to 1 connection from Homelab to VPS, 1 to many from VPS to internet
1. Any client with 0 setup: Port forwarding on your router => punch a hole
   through your firewall
    * Security: Requires hardening your homelab, which kind of sucks
    * Security: Without reverse proxy requires exposing service ports => Use
      only with reverse proxy and expose only 80/443
    * Security: Service publicly exposed -> use 2FA for service
    * ISP: not possible with CG-NAT
    * ISP: works if static public IP
    * ISP: If dynamic public IP use DDNS. Or use Domain Name API and a custom
      script or `ddclient`
    * 1 to many connection
1. Any client with 0 setup: 1 to 1 connection to Cloudflare. 1 to many from
   Cloudflare to public. Use either Cloudflare Tunnel or Opensource CLI.
    * Security: Secure out of the box but trusting 3rd party (I'm sure it has
      some issues I am not aware of)
    * Security: Service publicly exposed -> use 2FA for service
    * ISP: works with static/dynamic IP or CG-Nat
    * Cloudflare: TOS don't allow for any service e.g. videostreaming
    * 1 to 1 to many connection (I think)

Related:

* <https://kb.porkbun.com/article/190-getting-started-with-the-porkbun-api>
* <https://github.com/ddclient/ddclient>
* <https://github.com/anderspitman/awesome-tunneling>
* <https://serverfault.com/questions/896784/ssh-remote-port-forwarding-gatewayports-yes-which-machine-to-specify-on>
* <https://qbee.io/misc/reverse-ssh-tunneling-the-ultimate-guide/>
* [20240202132409](/20240202132409/) Publish a Homelab service to the internet with SSH and reverse proxy via VPS
* TODO add wireguard zet
* TODO add tailscale zet
* TODO add cloudflare tunnel zet
* TODO add port forwarding zet
