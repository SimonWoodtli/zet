# LINUX: create ProtonVPN daemon/service

1. Where installed?
`sudo which protonvpn`

1. Create file:
`sudo touch /etc/systemd/system/protonvpn-autoconnect.service`
1. Edit file: with this content
`sudo vi /etc/systemd/system/protonvpn-autoconnect.service`

```sh
[Unit]
Description=ProtonVPN-CLI auto-connect
Wants=network-online.target

[Service]
Type=forking
ExecStart=/usr/local/bin/protonvpn connect -f
Environment=PVPN_WAIT=300
Environment=PVPN_DEBUG=1
Environment=SUDO_USER=user

[Install]
WantedBy=multi-user.target
```
1. replace environment user with current Linux username
1. Reload daemon:
`sudo systemctl daemon-reload`
1. Connect startup:
`sudo systemctl enable protonvpn-autoconnect`

Tags:

    #linux #vpn #daemon #service #startup
