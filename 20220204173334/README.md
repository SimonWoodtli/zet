# LINUX: Raspian change username "pi"

## 1. Method

Create a new User and leave pi be or delete it. More information [here]

## 2. Method
1. set rootpw &
`sudo passwd root`
1. set CLI for boot startup `sudo raspi-config`
1. reboot&login to root
`sudo usermod -l newname pi -> newname=new chosen username`
1. move home directory
`usermod -m -d /home/newname newname`
1. change startup to boot GUI/desktop
`sudo raspi-config`
1. reboot then disable root pw
`sudo passwd -l root`

Tags:

    #linux #raspian #username

[here]: <https://www.raspberrypi.org/documentation/linux/usage/users.md>
