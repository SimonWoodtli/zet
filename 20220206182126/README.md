# How to turn off your desktop environment

Linux distributions can start and stop the graphical desktop in various ways. The exact method differs from distribution and among distribution versions. For the newer systemd-based distributions, the display manager is run as a service, you can stop the GUI desktop with the systemctl utility and most distributions will also work with the telinit command, as in:  
`sudo systemctl stop gdm` or `sudo telinit 3`

and restart it with:  
`sudo systemctl start gdm` or `sudo telinit 5`

On Ubuntu versions before 18.04 LTS, substitute lightdm for gdm.

## Methods of bringing down the GUI:

```sh
sudo systemctl stop gdm
sudo systemctl stop lightdm
sudo telinit 3
```

## Methods of bringing the GUI back up:

```sh
sudo systemctl start gdm
sudo systemctl start lightdm
sudo telinit 5
```

Tags:

    #GUI #linux #terminal #sysadmin #desktopEnvironment
