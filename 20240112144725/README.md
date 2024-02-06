# Why would you want to run your own DNS Server?

For privacy reasons and for convenience it may help to run your own DNS
server. One software that allows you to do so is bind9. There are different
ways to approach this:

1. On a custom router:
    1. With OpenWRT or pfSense you can do DNS host overwrites directly (this
       will give you local subdomains for your LAN server, but not more
       privacy). Some proprietary router firmwares also have this feature.
    1. With OpenWRT/pfSense you should also be able to use dnsmasq to achieve this.
    1. If you run other software on top of your router. You might want to add
       bind9 or another DNS server on it. This will give you privacy whilst
       using the internet at home or connected to it via VPN tunnel.
1. On a local LAN sever with bind9: Like mentioned before you get privacy and
   custom domains for your LAN as long as you are connected to your network.
1. On a VPS sever with bind9: Now you loose a little privacy and it might be a
   bit slower than a DNS on your LAN. But you can use it on any network
   conveniently.

## Privacy explained - Why should I care?

Your ISP may track your website visits, timestamps and metadata. If privacy is
your main concern. Then going with a public encrypted DNS is easier and more
effective (from my currently limited understanding of the topic).

## Convenience explained - Custom domain names why do I need that?

In short if you are running a local homelab and have some webapps and services
that have a web interface. You might want to access those sites more
conveniently. You probably don't want to deal with all those IPs and ports.

In long:

Running your own DNS server allows you to add any custom domain names and
subdomains that you want to assign to any of your homelab servers. In other
words your webapps/services can now be accessed with e.g https://sync.home
instead of http://192.168.0.5:8384.  However for this to work you also need a
proxy site since these services don’t run on port 80|443.

Why do we need both DNS and proxy? Because the domain name can only point to an
IP address. Since those webapps and services run on their own port we need a
proxy site too. That requires apache or nginx. At this point you can also
easily add certbot and pw authentication.

But not only homlab server would benefit from this also any remote server you
want to give a domain name would at least for you personally be reachable
without typing any pesky IPs or ports.

### What does not work

Some people recommend adding custom domain names that point to the IP of web
services to /etc/hosts of each of your machines. I tried this, but this method
is not working. Because you can only point the hostname of a given machine to
an alias hostname. But you cannot set any wildcards and hence no custom
subdomains can be used. The next best and simple solution seems to be dnsmasq.

So in /etc/hosts it would look like this:

```
192.168.0.5 hostnameOnMyPI aliasdomain
```

## My personal favorite methods

> 📝 It's complicated and all methods have their ups and downs. A public custom DNS
has an easy DDOS attack vector and you also trust a cloud provider with your
data.

The question you should ask yourself do I want to expose those services to the
public internet or keep them on my LAN? My main take away is don't run a custom
DNS service like `bind9`. But at least learn how to do it for fun.

If on LAN or later public:  
Use a free domain name registrar like DuckDNS and point it to a local IP. This
allows you to set a wildcard for subdomains.

Another benefit of doing this, is that you can add SSL certs for all your local
services without using certbot and having to worry that they might expire.
Whether you choose to make them public or not using nginx proxy manager, caddy
or your reverse proxy of choice.

* [20240113153426](/20240113153426/) Setup Service: DuckDNS with Auto SSL certs using reverse proxy and DNS validation for certs(DNS-01) for your homelab

Sidenote: My guess is that if I were to setup dnsmasq and have all the
subdomains setup that way, the auto SSL cert method would also be possible.

### Alternative methods I feel like are fine too

1. If on LAN: Setup dnsmasq on your dietpi or router. It's easier than to learn
   bind9 and setting up a custom DNS.
      1. TODO research more: Using encrypted DNS with stubby or DNS crypt.
      1. Easy-mode: Use a pihole not to adblock but just for an easy selfhosted DNS (bit bloated but it works)
1. If exposed public: Use tunneling methods like CloudFlare Tunnel or open
   source alternatives. [Checkout this repo][repo] for some projects. They work no
   matter what your ISP situation, even if behind CGNAT and don't require port
   forwarding on your router.

[repo]: <https://github.com/anderspitman/awesome-tunneling>

Related:

* <https://privacytools.io/providers/dns/>
* <https://cloudflare.com/ssl/encrypted-sni/>
* <https://stackoverflow.com/questions/20446930/how-to-put-wildcard-entry-into-etc-hosts>
* <https://stackoverflow.com/questions/45967872/how-to-setup-local-domain-in-local-network-that-everyone-can-see>
* <https://github.com/anderspitman/awesome-tunneling>
* [20240113153426](/20240113153426/) Setup Service: DuckDNS with Auto SSL certs using reverse proxy and DNS validation for certs(DNS-01) for your homelab
* TODO add zet how to setup CloudFlare tunnel to expose LAN services publicly
* TODO add zet how to setup tunnel.pyjam.as to expose LAN services publicly

* TODO add zet how to setup dnsmasq, research: if auto SSL certs would work with nginx proxy manager
* TOdo add zet how to configure router with DNS host overwrites
* TODO add zet how to setup bind9 on LAN and on VPS
* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240110232911](/20240110232911/) LAN Server Install: Setup and Install dietpi

Tags:

      #linux #server #homelab #LAN #DNS #SSL #router #networking #important
