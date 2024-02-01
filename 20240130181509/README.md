# Restart podman containers after reboot with systemd user service

## Method 1: (deprecated, but still totally fine)

> 🧐 Normally user services start when the user logs in and stop when the user
> logs out. If you want a user service to start after a reboot even if the user
> is not logged in or keep running after the user logs out enable lingering.
> [See wiki][wiki] for more information.

E.g syncthing app

1. Make sure your container is running
1. `mkdir -p ~/.config/systemd/xnasero/`
1. Create: `podman generate systemd --new containerNameOfSyncthing > ~/.config/systemd/xnasero/syncthing.service`
1. Enable: `systemctl --user daemon-reload && systemctl --user enable ~/.config/systemd/xnasero/syncthing.service`
1. Make service persistent outside of user login session: `loginctl enable-linger xnasero`
1. More Info: <https://www.redhat.com/sysadmin/podman-features-3>

## Method 2: Quadlets

1. More Info: <https://www.redhat.com/sysadmin/quadlet-podman>

[wiki]: <https://wiki.archlinux.org/title/systemd/User>

Tags:

      #linux #container #podman #sysadmin #devops #systemd
