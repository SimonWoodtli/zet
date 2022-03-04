# LINUX: Raspian OS looks and optimize

1. `sudo apt install arc-theme breeze-cursor-theme`, go to: 'main menu editor'
there add: 'theme and appearance settings', now change it under 'preference'
1. `sudo vi /boot/config.txt` change arm_freq=2000 and arm_freq_min=1000 
and under_voltage=6
1. `sudo vi /usr/lib/raspi-config/cmstart.sh` comment the xcompmr line then 
add xcompmgr -c -r10 -F -f -D5 -C -o0.8
1. `sudo apt install gnome-software flatpak`
1. `flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`
1. `sudo apt install gnome-software-plugin-flatpak nemo`
1. `sudo apt install lxsession-default-apps` go to main menu editor add it and 
change nemo to default file manager

Tags:

    #linux #arm #raspian #raspberryPi #appearance #performance #optimization
