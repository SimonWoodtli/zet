# HomeLab Server

Have a fast Cache NVME/SSD: Find out a way to first store new files downloaded on NVME or SSD and if they are not used for 5days or more copy them to HDD.

Redundancy: Mergerfs+[Snapraid] => +HDD size can be different, +HDD can be added later on
Redundancy: ZFS => all drives need to be same size, can't add more drives after setting it up

Webapps: All as Docker containers either as LXC containers per app . Or the other idea I have is to have a VM with ubuntu server. Then install portainer on it to manage all the docker containers. (more research needed to which approach is better, maybe also depends on the app itself I plan to host as a docker container or LXC.

E.g. adguard/pihole in a LXC container => or a docker container inside the ubuntu VM

https://github.com/awesome-selfhosted/awesome-selfhosted
https://github.com/trapexit/mergerfs
 

[Snapraid]<https://www.snapraid.it/>
