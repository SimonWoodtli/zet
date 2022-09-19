# Nix Package Manager: Cheatsheet

* List Installed packages `nix-env -q`
* Install Packages `nix-env -iA nixpkgs.packagename`
* Erase Packages `nix-env -e packagename`
* Update All Packages `nix-env -u`
* Update Specific Packages `nix-env -u packagename`
* Hold Specific Package `nix-env --set-flag keep true packagename`
* List Backups (Generations) `nix-env --list-generations`
* Rollback to Last Backup `nix-env --rollback`
* Rollback to Specific Generation `nix-env --switch-generation #`

Checkout their [manual] or get some info with `man nix` or `man nix-env`

[manual]<https://nixos.org/manual/nix/stable/>
