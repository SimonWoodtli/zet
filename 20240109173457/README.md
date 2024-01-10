# Server Service: Add mumble server

> 🧐 easy-mode: install older version `apt install mumble-server` and then just open the port.

1. Just install with `apt install mumble-server` and it runs the daemon
   automatically and works. (Although the version is a bit outdated)
1. open port `ufw allow 64738`
1. Connect from your client and be happy :)
1. TODO: add password protection to connect to server

## Don't do this currently WIP, and not working :)

1. Install dependencies: `apt install ...`
1. [Install latest mumble-server][mumble] on your local machine and copy it to your server
1. Change permission of .ini config file to root: `chown `whoami`:`whoami` /etc/mumble/mumble-server.ini`
1. Run mumble: `mumble-server -ini ~/.mumble-server.ini -supw <your_password>`
1. open port `ufw allow 64738`
1. Check if running: `ps -ef | grep mumble-server`

#### TODO: Figure out how to get my mumble-server binary to work: (I tried installing mumble-server apt pkg to see what i might miss)

* murmurd is the daemon binary
* mumble-server.service is the systemd service then starts the murmurd binary
* where is mumble-server actually invoked? I guess in the murmurd bin
* where is mumble-server binary installed with default package?
* /etc/init.d/mumble-server is a startup bash script that invokes murmurd (from what i can tell form short glimpse over code)
* mumble-server install seems to be a bit more complicated :)
* I don't want to mess around with the default package files as they would be
  overwritten on update and it's really bad practice anyways. However I might
  need more than just mumble-server to get this to work. Maybe I can copy some
  more config files from the mumble-server apt pkg and try again in the future.
* `systemctl status mumble-server.service`

[mumble]:<https://github.com/SimonWoodtli/mumble>

Related:

* <https://github.com/mumble-voip/mumble/tree/master/docs>
* <https://landchad.net/mumble/>

Tags:

      #linux #server #sysadmin #voip #voice
