# Docker Setup

In my install-docker script I made sure to run docker.service rootless.

* To check the current status: `systemctl --user --no-pager --full status docker.service`
* To enable docker.service at startup: `sudo loginctl enable-linger $(whoami)`

Related:

* <https://docs.docker.com/engine/security/rootless/>

Tags:

    #linux #container #virtualization #application #docker
