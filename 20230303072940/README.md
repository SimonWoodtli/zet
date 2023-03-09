# NixOS install with LUKS encryption

> 🧐 Checkout [cha 9.][9.] for a quick install.

## 1. Format Disk

We will format with 4 partitions:

1. boot
2. OS
3. documents/data
4. swap

> 🧐 If your HDD is > 2TB use gdisk/parted with GPT. If less use MBR with fdisk

### 1.1 Create Partition Table and Partitions

**Parted:**

> 🧐 Here we use UEFI so the first nixos partition starts from 512MiB, If legacy BIOS: 1MiB

cmd explained: `parted -- mkpart partition-label partition-type
startpoint[MiB|%] endpoint[MiB|%]`  
The Order of executing parted mkpart cmds matters, as the partition number
starts with 1 and goes to 4 in this case.

1. create partition table: `parted /dev/sdXX -- mklabel gpt`
1. If UEFI, create boot partition: `parted /dev/sdXX -- mkpart ESP fat32 1MiB 512MiB`
1. create NixOS partition: `parted /dev/sdXX -- mkpart primary ext4 512MiB 27%`
   , if legacy BIOS it'd be 1MiB instead of 512MiB
1. create Document/Data partition: `parted /dev/sdXX -- mkpart secondary ext4 27% 99%`
1. create Swap partition: `parted /dev/sdXX -- mkpart swap linux-swap 99% 100%`

If UEFI, set esp flag to on : `parted /dev/sdXX -- set 1 esp on`, 1 refers to
first partition
If BIOS, set boot flag to on: `parted /dev/sdXX -- set 1 boot on`

**Gdisk:**

I prefer `parted` as fdisk/gdisk is a TUI and writing documentation for
instructions is annoying. But those main hotkeys are only needed to get the
same result as with `parted`.

* to start TUI: `gdisk /dev/sdXX`

Main hotkeys:

* help: `?`
* delete partition: `d`
* delete all partitions and create GPT: `o`
* new partition: `n`
* write: `w`
* name/label partition: `c`

### 1.2 Create Filesystems for each Partition:

1. If UEFI, boot partition: `mkfs.fat -F 32 -n boot /dev/sdXX1`
1. swap partition: `mkswap -L swap /dev/sdXX4`

**Optional:** Only run the next commands to create a file system for your
primary/secondary partitions if you do not want to encrypt them.

1. primary partition: `mkfs.ext4 -L nixos /dev/sdXX2`
1. secondary partition: `mkfs.ext4 -L data /dev/sdXX3`

### 1.3 Encrypt primary/secondary Partitions:

> 🧐 If you use the same luks password for multiple disks, partitions and want them to autostart at boot time, you only need to enter it for one disk/partition

1. encrypt primary: `cryptsetup -y -v luksFormat --label crypto-nixos /dev/sdXX2`
1. encrypt secondary: `cryptsetup -y -v luksFormat --label crypto-data /dev/sdXX3`
1. decrypt primary: `cryptsetup open /dev/sdXX2 primary`, primary is just the
   name to mount the container later
1. decrypt secondary: `cryptsetup open /dev/sdXX3 secondary`
1. create fs primary: `mkfs.ext4 -L nixos /dev/mapper/primary`
1. create fs secondary: `mkfs.ext4 -L data /dev/mapper/secondary`

## 2. Mount partitions

1. create dir: `mkdir /mnt`, mount: `mount /dev/disk/by-label/nixos /mnt`
1. If UEFI, create dir: `mkdir /mnt/boot/efi`, mount: `mount /dev/disk/by-label/boot /mnt/boot/efi`
1. If BIOS, create dir: `mkdir /mnt/boot`, mount: `mount /dev/disk/by-label/boot /mnt/boot`
1. create dir: `mkdir -p /mnt/home/xnasero/data`, mount: `mount /dev/disk/by-label/data /mnt/home/xnasero/data`

## 3. Initial NixOS Configuration

1. generate config files: `nixos-generate-config --root /mnt`
    1. build/customize config files in /mnt/etc/nixos/*
    1. or rm auto generated config files and curl my preconfigured files: `curl ...`

## 4. Install OS

> 🧐 If you want to make changes to your config files after the first install
don't use `nixos-install` instead use `nixos-rebuild switch` or `nixos-rebuild
build`

1. `nixos-install`, give a root pw at the end of install
2. reboot into your new OS 🎉

## 5. First Boot

1. add current stable package list (update version, if newer exist): `sudo
   nix-channel --add https://nixos.org/channels/nixos-22.11`
1. update channels: `sudo nix-channel --update`

## 6. Install Home Manager

> ⚠️ If you plan to use flakes skip the steps described and go to [cha. 7][7.]

1. if you add home-manager as a NixOS Module into your configuration.nix file,
   add packages for home manager: `sudo nix-channel --add
   https://github.com/nix-community/home-manager/archive/release-22.11.tar.gz
   home-manager`
1. Add to /etc/nixos/configuration.nix:

```
  imports =
    [
      ./hardware-configuration.nix
      <home-manager/nixos>
    ];

  home-manager.users.${user} = { pkgs, ... }: {
    home.stateVersion = "22.11";
    home.packages = with pkgs; [ btop ];
  };
```

You can also add home-manager as a standalone but I wouldn't recommend it.  
For more details: See [home manager install][home-manager]

## 7. Install Flakes

1. Add to /etc/nixos/configuration.nix:

```
  nix = {
    package = pkgs.nixFlakes;
    extraOptions = "experimental-features = nix-command flakes";
  };
```


2. Create and init flake dir: `mkdir flake && cd $_ && nix flake init`
3. Add to flake/flake.nix:

```
{
  description = "A very basic flake";

  inputs = {
  # if you want unstable change nixos-22.11 with nixos-unstable
    nixpkgs.url = "github:nixos/nixpkgs/nixos-22.11";
  };

  outputs = { self, nixpkgs }:
    let
    ## change system to your architecture
      system = "x86_64-linux";
      pkgs = import nixpkgs {
        inherit system;
        config.allowUnfree = true;
      };
      lib = nixpkgs.lib;
    in {
      nixosConfigurations = {
      ## here xnasero is the name of the NixOS System Configuration. You can
      have multiple system configurations stored in the same flake.nix. But
      then you need to specify the one you want to use like so:
      ## `sudo nixos-rebuild switch --flake .#xnasero`
      ## `sudo nixos-rebuild switch --flake .#anotherSysConfig`
        xnasero = lib.nixosSystem {
          inherit system;
          modules = [ ./configuration.nix ];
        };
        #anotherSysConfig = lib.nixosSystem {
        #  inherit system;
        #  modules = [ ./configurationForTheNewSysConfig.nix ];
        };
      };
    };
}
```

> 🧐 If you only have one NixOS system configuration defined in flake.nix you can use .# like so: `nixos-rebuild switch --flake .#`

4. copy config files from /etc: `cp /etc/nixos/* .`
    1. Now you no longer need to change the /etc/nixos/config files instead you
       change them directly in your flake
5. generate flake.lock file: `nix flake lock`
6. update configuration: `sudo nixos-rebuild switch --flake .#` (This will only
   read programs from flake.lock and update them according to the version that
   is mentioned in flake.lock)
7. update flake: `nix flake update` (This will check programs in flake.lock and
   check if there is newer versions available, if so flake.lock gets
   overwritten with new versions.)

## 8. Home Manager with Flakes

You have two options, either you add home-manager separately or within your
NixOS System Configuration (./configuration.nix)

**Option 1:** NixOS Configuration Module

1. add home-manager to your inputs/outputs within the same NixOS System
   Configuration "xnasero": `vim flakes.nix`

```
{
  description = "A very basic flake";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-22.11";
    home-manager = {
      url = github:nix-community/home-manager;
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { self, nixpkgs, home-manager, ... }:
    let
      system = "x86_64-linux";
      pkgs = import nixpkgs {
        inherit system;
        config.allowUnfree = true;
      };
      lib = nixpkgs.lib;
      user = "xnasero";
    in {
      nixosConfigurations = {
        xnasero = lib.nixosSystem {
          inherit system;
          modules = [ 
            ./configuration.nix 
            home-manager.nixosModules.home-manager {
              home-manager.useGlobalPkgs = true;
              home-manager.useUserPackages = true;
              home-manager.users.${user} = {
                imports = [ ./home.nix ];
              };
          }
          ];
        };
      };
    };
}
```

2. To update everything home-manager and all configs: `sudo nixos-rebuild
   switch --flake .#xnasero`

**Option 2:** Separate (don't recommend)

1. add home-manager to your inputs/outputs with new NixOS System Configuration:
   `vim flakes.nix`

```
  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-22.11";
    home-manager = {
      url = "github:nix-community/home-manager";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { self, nixpkgs, home-manager, ... }:
    let
      system = "x86_64-linux";
      pkgs = import nixpkgs {
        inherit system;
        config.allowUnfree = true;
      };
      lib = nixpkgs.lib;
      user = "xnasero"; in {
      nixosConfigurations = {
        xnasero = lib.nixosSystem {
          inherit system;
          modules = [ ./configuration.nix ];
        };
        hmConfig = {
        ## xnasero is just the name for your NixOS System Configuration. Same as before.
          xnasero = home-manager.lib.homeManagerConfiguration {;
            inherit system pkgs;
            username = "${user}";
            homeDirectory = "/home/{user}";
            configuration = {
              imports = {
              # if you don't host your flake in a git repo
              #  /home/${user}/.config/nixpkgs/home.nix
              # if you host your flake in a git repo
                ./home.nix
              };
            };
          };
        };
      };
    };
```

2. touch file: `touch home.nix` and add content:

```
{ config, pkgs, ... }:

{
  # Home Manager needs a bit of information about you and the
  # paths it should manage.
  home.username = "xnasero";
  home.homeDirectory = "/home/xnasero";

  # This value determines the Home Manager release that your
  # configuration is compatible with. This helps avoid breakage
  # when a new Home Manager release introduces backwards
  # incompatible changes.
  #
  # You can update Home Manager without changing this value. See
  # the Home Manager release notes for a list of state version
  # changes in each release.
  home.stateVersion = "22.11";

  # Let Home Manager install and manage itself.
  programs.home-manager.enable = true;
}
```

3. Build it: `nix build .#hmConfig.xnasero.activationPackage`
4. Activate it: `cd result && ./activate`

Why shouldn't you use this method? To update you now have to do 3 things:

1. `sudo nixos-rebuild switch --flake .#xnasero`
1. `nix build .#hmConfig.xnasero.activationPackage`
1. `cd result && ./activate`

## 9. Auto Install with GitHub hosted Flake on a new Machine

1. Boot into live NixOS CD
1. `sudo su`
1. Partition your drive, see [cha. 1.][1.]
1. Mount the required partitions, see [cha. 2.][2.]
1. Get git: `nix-env -iA nixos.git`
1. Clone flake: `git clone https://github.com/SimonWoodtli/nixos-config.git /mnt/etc/nixos`
1. Install it: `cd` into repo and `nixos-install --flake .#<NixOSSysConfig`
1. Reboot
1. Bug, need to rm config: `sudo rm -r /etc/nixos/configuration.nix`
1. Move your flake repo to a place in your /home/<user> dir, change owner too.
   Or just simply reclone it
1. Congratulations you are all setup! 🎉

[1.]: <#1-format-disk>
[2.]: <#2-mount-partitions>
[7.]: <#7-install-flakes>
[9.]: <#9-auto-install-with-github-hosted-flake-on-a-new-machine>
[home-manager]: <https://github.com/nix-community/home-manager#installation>

Related:

* <https://nixos.org/manual/nixos/stable/>
* <https://search.nixos.org/packages>
* <https://zero-to-nix.com/>
* <https://nix-community.github.io/home-manager/index.html>
* <https://nix-community.github.io/home-manager/options.html>

* <https://github.com/SimonWoodtli/nixos-config>
* <https://jdisaacs.com/blog/nixos-config/>
* <https://github.com/wiltaylor/dotfiles>
* <https://serokell.io/blog/practical-nix-flakes>
* <https://lantian.pub/en/article/modify-website/nixos-why.lantian/>
* <https://xeiaso.net/blog/nix-flakes-1-2022-02-21>
* <https://github.com/MatthiasBenaets/nixos-config>
* <https://www.youtube.com/watch?v=AGVXJ-TIv3Y>
* <https://www.youtube.com/playlist?list=PL-saUBvIJzOkjAw_vOac75v-x6EzNzZq->

Tags:

    #linux #nixos #tutorial
