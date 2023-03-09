# Install nix package manager inside container

since containers don't have systemd or open-rc you cannot install nix with a daemon. That means you need to use the single user install script.

`sh <(curl -L https://nixos.org/nix/install) --no-daemon`

Tags:

    #nix #package #software #install #container

