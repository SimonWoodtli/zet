# NixOS cheatsheet

## install

* rebuild system: `nixos-rebuild build`
* rebuild system and switch: `nixos-rebuild switch` 

## nix-env

I prefer setting up flakes and use `nix profile` instead of `nix-env`.

> ⚠️  Using `nix-env` permanently modifies a local profile of installed packages. This must be updated and maintained by the user in the same way as with a traditional package manager, foregoing many of the benefits that make Nix uniquely powerful. Using `nix-shell` or a NixOS configuration is recommended instead.

* list all programs installed with nix-env: `nix-env -q`
* install with nix-env: `nix-env -iA nixos.XX`
* uninstall with nix-env: `nix-env --uninstall XX`

## nix-shell

> 🧐 A `nix-shell` will temporarily modify your \$PATH environment variable. This can be used to try a piece of software before deciding to permanently install it.

## general

* update system: `nix-channel --update && sudo nixos-rebuild switch --upgrade`
* checkout config man: `man configuration.nix`
* checkout home manager man: `man home-configuration.nix`
* rm old unused packages: `sudo nix-collect-garbage -d`
* list all generations: `nix-env --list-generations`
* switch generation: `nix-env --switch-generation`

