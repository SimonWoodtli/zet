# Fedora Silverblue cheatsheet

> 🧐 `rpm-ostree` doesn't need sudo to install software as it cannot write into the immutable / dir. The only exception is when you want to update the 'base' software packages that fedora ships with and that are installed in /.

* update software: `sudo rpm-ostree upgrade` (test does this cmd include updating flatpaks?)
* rollback to prev. version: `rpm-ostree rollback`
* install software:
    * flatpak (for gui) `flatpak install flathub <pkg>`
    * rpm-ostree (for cli, I prefer nix) `rpm-ostree install --apply-live <pkg>`
    * toolbox (container for dev. env., I prefer podman/distrobox)
    * nix (I personally use it for most software)
    * distrobox
    * nix/distrobox are not shipped with Silverblue
* uninstall software:
    * `rpm-ostree uninstall <pkg>`

apply installed software without reboot: `rpm-ostree ex apply-live`

Related:

* <https://docs.fedoraproject.org/en-US/fedora-silverblue/>

Tags:

    #linux #fedora #silverblue #immutable
