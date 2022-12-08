# Cheatsheet Manage Wireguard VPN connections

> 🧐 Gnome 43 does not support to manage Wireguard connections via settings (GUI). Use the Network Managers cli `nmcli` to do so or get the wireguard-vpn-extension from the extension manager.

## nmcli commands

\$1 == yourWireguardProfile, the name from the imported profile

* check network status: `nmcli`
* import profile: `nmcli connection import type wireguard file <yourWireguardProfile.conf>`
* delete profile: `nmcli connection delete <yourWireguardProfile.conf>`
* check profile settings: `nmcli --show-secrets connection show $1`
* connect to profile: `nmcli --show-secrets --ask connection up $1`
* disconnect from profile: `nmcli connection down $1`
* enable auto connect from profile: `nmcli connection modify $1 connection.autoconnect yes`
* disable auto connect from profile: `nmcli connection modify $1 connection.autoconnect no`
* change profile name: `nmcli connection modify $1 connection.interface-name <newProfileName>`
* change profile connection id: `nmcli connection modify $1 connection.id <newProfileID>`
* change settings from profile: `nmcli connection modify $1 <setting.property> <newValue>`

Related:

* <https://blogs.gnome.org/thaller/2019/03/15/wireguard-in-networkmanager/>

Tags:

    #linux #wireguard #vpn #profile #management #networkManager #nmcli
