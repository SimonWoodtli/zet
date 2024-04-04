# Systemd Cheatsheet

* list all available services: `systemctl list-units -t service --all`
* list active services: `systemctl list-units -t service`
* list user services related to xdg: `systemctl list-units --user 'xdg*'`
* start and enable service: `systemctl enable foo.service --now`
* reload service (after editing file): `systemctl daemon-reload`
