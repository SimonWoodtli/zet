# Linux System units/service VS. User units/service

> 📝 To make it easier I still refer to system units as a service. Although
> system services and system units are not the same systemd uses units (to be
> precise, see article)

* System services are running as root
* User services are running as user
* They both can be managed with `sytemctl`
* If you make changes to them 1. stop the service 2. daemon-reload the service 3. enable the service

## User service

> 🧐 Many services you might consider to run don't require root so run them as
> user services.

Locations:

* ~/.config/systemd/YourUsername: This dir contains the user unit files (if you
  enable the service they get symlinked to '~/.config/systemd/user'
* ~/.config/systemd/user: Managed by systemd, any unit in here means they are
  already enabled (if you disable them they get deleted here)

In order to handle `systemctl` with user services you don't need root
privileges but a user flag e.g. `systemctl --user enable foo.service --now`

* In some cases I noticed it's also necessary to give the path of the
  foo.service file e.g. `systemctl --user enable
  ~/.config/systemd/xnasero/foo.service --now`

## System service

> 🧐 Don't make changes to service files that are managed via package manager.
> Because they just get overwritten on the next update. Instead add them to
> /etc/systemd/system because this location has priority over the others.

> 📝 In most cases for individually created unit files add them to /etc/systemd/system

Locations:

* /etc/systemd/system/: This dir contains custom/individual (user-defined) unit files (they overwrite any other unit files in the other location)
* /usr/lib/systemd/system/: This dir contains service files installed by packages
* /lib/systemd/system/: This dir contains vendor-supplied unit files
* /run/systemd/system/: This dir contains run-time unit files (they get dynamically created at run-time by systemd and deleted after reboot)

In order to handle `sytemctl` with system services you need root privileges
e.g. `sudo systemctl enable foo.service --now`

Related:

* <https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files>

Tags:

      #linux #systemd #systemctl #service #sysadmin
