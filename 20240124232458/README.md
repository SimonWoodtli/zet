# Syncthing configuration and caveats

> ⚠️  There is no server client model with syncthing. Each place you install
> syncthing is both server and client.

* If you install syncthing via package manager by default it creates a systemd
  system service which runs syncthing as root. You should definitely fix that.
  However I prefer to run it via docker/podman, so if you installed docker to run
  rootless that is not an issue.
* By default syncthing will sync over the internet via some global servers/relays they
  host.
* By default it syncs across all connected devices (Send and Receive). Meaning
  if you delete a file/folder in one device it deletes it everywhere.
* Where syncthing really shines is if you have devices that share a folder but
  both make changes to it. With `rsync` this is getting complicated. But I
  still like to use `rsync` for any VPS servers because on them I don't store
  any data that requires dynamic changes from both sides.

> 📝 In an ideal world you have a dedicated own backup server for syncthing
> with raid 1 (this machine does not require lots of resources and may be quite
> shitty). And all your other computers like your main server, desktop, laptop
> phone also run syncthing. Alternatively your "backup" server is just a VM in
> your main server. And you'd have a backup syncthing running on it and a
> normal syncthing running wherever your services/apps run too.

## 1. Actions: Settings

> ⚠️ For each syncthing instance/device you setup you have to configure these 4
> steps.

* Checkout the /etc/conf.d/syncthing file to customize syncthing without GUI

1. General: Edit Folder Defaults
    1. Change folder path
    1. Change Versioning (none or trash depending on machine)
1. Connections: Enable LAN only sync(way faster): Go to settings and disable 'Global Discovery' and 'Enable Relaying'

### 1.1 Actions: Advanced

> 🧐 If you run a docker/podman container and you do so with host networking enabled
> you won't get that Red alert about remote access and password. [See docs][syncthing]

However for all installs via package manager where you secure the access with
nginx http auth or otherwise you can disable this message remote access warning:

* GUI: Enable 'Insecure Admin Access'

## 2. Auto start syncthing after reboot

* If you installed syncthing via package manager follow the auto start
  [official docs][docs] of how to create a systemd user service
* If you installed syncthing with docker you are good, the docker daemon takes
  care of it just make sure to have 'restart: unless-stopped’ in your compose
  file.

Currently podman cannot run syncthing rootless. So you have to run it with
`sudo podman ...` and also add `--priviliged`. Because of that you also need a
systemd system service. However once the uid/gid issue is fixed it should work
rootless. In the meantime: (rm this once fixed)

1. Run syncthing container(root): `sudo podman compose up -d`
1. Create: `sudo sh -c 'printf "%s\n" "$(sudo podman generate systemd --new syncthing)" > /etc/systemd/system/syncthing.service'`
1. Enable: `sudo systemctl daemon-reload && sudo systemctl enable syncthing.service`

* If you installed syncthing with podman you need to create a systemd user service:
    1. Method 1: (deprecated, but still totally fine)
    1. Make sure your container is running
    1. `mkdir -p ~/.config/systemd/xnasero/`
    1. Create: `podman generate systemd --new containerNameOfSyncthing > ~/.config/systemd/xnasero/syncthing.service`
    1. Enable: `systemctl --user daemon-reload && systemctl --user enable ~/.config/systemd/xnasero/syncthing.service`
    1. More Info: <https://www.redhat.com/sysadmin/podman-features-3>
    1. Method 2: Quadlets
    1. More Info: <https://www.redhat.com/sysadmin/quadlet-podman>

> 📝 There is also a GUI app for syncthing as a [flatpak]. I haven't used it as
> I see no need for it but it's there.

## 3. Adding devices

Generally speaking just copy/paste the device-id or scan the QR code.

One imported thing to note when you add devices in 'Advanced' in addresses you
can set a local IP. So for your server or devices in your LAN with static IPs
it's good to add them there. It makes finding devices and connecting to other
devices easier. e.g 'tcp://192.168.1.32:22000'. You can also use DNS records if
you prefer.

### Container 'Disconnected' connection problem

If you just start adding them up and leave the address as 'dynamic' on every
device it won't work with containerized syncthing instances. If they all run on
the host it works just fine with just 'dynamic', but not with a virtual docker
network.

To fix this:
Add the remote device first on the instance that does not run with docker. Then
instead of leaving the address 'dynamic' change it to the local IP (ofc. makes
more sense if the IP is actually static too). Maybe you even have a duckDNS
record for it either way it should look something like:
‘tcp://192.168.1.32:22000’ or ‘tcp://foo.duckdns.org’. Now on the docker
instance you can still add the other device and leave it 'dynamic' they should
now find each other. If all instances run via docker I guess you'd need to set
IPs everywhere (have not tested that). My guess is also that the IPs don't have
to be static it's just for making this initial handshake and when more devices
are connected syncthing should be having less of this "Disconnected" issues.

## 4. Adding Folders

For each folder you create you can have settings on how to sync between
devices. This is great and gives you a lot of flexibility. Some folders require
a simple sync solution whilst with others it makes more sense to choose a
backup syncing system.

For each folder you share you have to ask yourself do I make changes on every
device(1:1 sync)? Or do I actually only ever make changes on one device but
never on the other (backup system).

### Architecture: dynamic 1:1 sync

E.g a folder that is shared between your phone and your desktop computer and
you expect both places to be able to make changes. Like a folders for photos.
In this case 'Send and Receive' is the way to go. And both devices would run
with 'Send and Receive'.

About the versioning: You can decide if you need extra versions on your phone
or just have them on your personal desktop computer. I prefer to not have any
versioning going on on the phone to save space and enable versioning only on
other devices.

### Architecture: Backup sync

E.g a folder that is only ever used and changed on a device, but never on the
other. For example you want to share your ~/Downloads folder on your personal
desktop with your server. Then you'd set the desktop to 'Send only' and the
servers folder settings to 'Receive only'

About the versioning: Again enabling it on both places is probably overkill.
And you'd want to  decide which machine has more space available for it.

However for this architecture I actually still prefer `rsync` with a cronjob.
It also has the advantage that if your "client" has corrupted data it's not
pushing that corrupted data to your backup system.

### Folder Settings

1. General: (tip FolderPath can be adjusted to another default path)
   1. Give it a Label/Name
1. Sharing: Select all the devices that share this folder
1. File Versioning: Trash Can(keeps files for a while even after deleting them)
1. Ignore Patterns: just like any other ignore file, will ignore them for sync
1. Advanced: Enable 'Watch for Changes' like `entr`
1. Advanced: Folder Types 'Send & Receive', 'Send only' and 'Receive only'

[docs]: <https://docs.syncthing.net/users/autostart.html#linux>
[syncthing]: <https://github.com/syncthing/syncthing/blob/main/README-Docker.md>
[flatpak]: <https://flathub.org/apps/me.kozec.syncthingtk>

Related:

* [20240125154231](/20240125154231/) Linux System units/service VS. User units/service
* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240110174327](/20240110174327/) Server Service: Add syncthing

Tags:

      #linux #syncing #files #rsync #server #config #setup
