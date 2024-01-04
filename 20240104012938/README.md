# Server Service: Add and Secure Docker

## Install Docker

1. Install docker
2. Add users who neeed docker to docker group: `usermod -aG docker xnasero`

## Secure Docker

> 🧐 linuxserver.io images are well maintained and updated frequently.

* Major issue: Don't expect the docker image of any given service/app to be up
  to date. That really depends on the maintainer of the image and how he set it
  up.
* Major issue: Even if the image is well maintained and updated you still need
  to redeploy your container as they are immutable. Use `watchtower` for
  dealing with this.
* Major issue: Docker containers bypass your `ufw` firewall settings. Even if
  you don't allow any incoming traffic a running docker app will still be
  allowing traffic see `ss -ltpn`. That is because both `ufw` and the docker
  image use iptables in the backend. And the docker iptables take priority thus
  over writing your servers `ufw` settings.

* TODO: Test this out see if those fixes actually work:
    * FIX Method 1: Using straight iptables instead allows you to actually have
      control over docker port-wise. Then disable iptables update by docker in
      docker config by –iptables=false parameter and manage all necessary
      setting for docker manually.
    * FIX Method 2: Another Way to Fix: instead of using `ufw` use `firewalld`

Related:

* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104134254](/20240104134254/) Server Security and Setup: Make it cosy
* [20240104124550](/20240104124550/) Server Security: Additional SSH Hardening
* [20240104130222](/20240104130222/) Server Security: Config ufw Firewall

Tags:

    #linux #docker #sysadmin #server #vps #containerization #security #firewall
