# Install and Setup Fedora Silverblue

# Install

> 🧐 I tried manually formatting my HDD, their live-installer GUI sucks. And
you cannot manually prepare your disk formated in the way you like. Their Live
ISO doesn't even come with a terminal so I had to boot into another live ISO in
order to test that out, but to no avail.

1. Bootup Live ISO
1. Go through install process with defaults and automatic partitioning
1. reboot

## Setup

1. Install Nix Package Manager, but cause of SELinux you have to do
   a [workaround]. I hope the regular nix install with `sh <(curl -L
   https://nixos.org/nix/install) --daemon` or `curl --proto '=https' --tlsv1.2
   -sSf -L https://install.determinate.systems/nix | sh -s -- install` will
   integrate that soon.
1. Enable flathub.org `flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`

[workaround]: <https://github.com/dnkmmr69420/nix-with-selinux>


Related:

* <https://docs.fedoraproject.org/en-US/fedora-silverblue/>

Tags:

    #linux #fedora #tutorial
