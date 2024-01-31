# Add Service: Uptime Kuma

> 📝 Either install docker or podman. I prefer the latter because it runs rootless,
daemonless, doesn't overwrite firewall rules and has a more unix like modular
approach. Above all they published their software with Apache 2.0 which is great.

1. Install: `apt install podman podman-compose -y`
1. Test: `podman run hello-world`
1. Create dir: `mkdir -p ~/Containers/uptime-kuma/data`
1. Create compose: `vi ~/Containers/uptime-kuma/compose.yml`

```
version: "3.1"

services:
  uptime-kuma:
    image: docker.io/louislam/uptime-kuma:1
    container_name: uptime-kuma
    volumes:
      - /home/xnasero/Containers/uptime-kuma/data:/app/data
      #- /var/run/docker.sock:/var/run/docker.sock #use this if you want to monitor docker apps that are not exposed to the host
    ports:
      - 3001:3001
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
```

5. Run it: `podman-compose up -d --force-recreate` (or `podman compose`)
5. Create a systemd user service:
    * [20240130181509](/20240130181509/) Restart podman containers after reboot with systemd user service
5. Open ports: `ufw allow 80; ufw allow 443`
5. Add a proxy site with nginx to redirect 0.0.0.0:3001 to your public_IP:80 (or use Traefik/Caddy)
5. Add a SSL cert: `certbot --nginx`

## Next Steps

* [20240131005312](/20240131005312/) Setup Service: Uptime Kuma

Related:

* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240110232911](/20240110232911/) LAN Server Install: Setup and Install dietpi

Tags:

    #linux #server #monitoring #dashboard #uptime #networking #container
