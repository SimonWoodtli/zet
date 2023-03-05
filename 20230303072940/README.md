# NixOS install

## 1. Format Disk

We will format with 4 partitions:

1. boot
2. OS
3. documents/data 
4. swap

If your HDD is > 2TB use gdisk/parted with GPT. If less use MBR with fdisk


### 1.1 Create Partition Table and Partitions

Parted:

> 🧐 Here we use UEFI so the first nixos partition starts from 512MiB, If legacy BIOS: 1MiB

cmd explained: `parted -- mkpart partition-label partition-type startpoint[MiB|%] endpoint[MiB|%]`  
The Order of executing parted mkpart cmds matters, as the partition number starts with 1 and goes to 4 in this case.

1. create partition table: `parted /dev/sdXX -- mklabel gpt`
1. If UEFI, create boot partition: `parted /dev/sdXX -- mkpart ESP fat32 1MiB 512MiB`
1. create NixOS partition: `parted /dev/sdXX -- mkpart primary ext4 512MiB 27%` , if legacy BIOS it'd be 1MiB instead of 512MiB
1. create Document/Data partition: `parted /dev/sdXX -- mkpart secondary ext4 27% 99%`
1. create Swap partition: `parted /dev/sdXX -- mkpart swap linux-swap 99% 100%`

If UEFI, set esp flag to on : `parted /dev/sdXX -- set 1 esp on`, 1 refers to first partition
If BIOS, set boot flag to on: `parted /dev/sdXX -- set 1 boot on`

Gdisk:

I prefer `parted` as fdisk/gdisk is a TUI and writing documentation for instructions is annoying. But those main hotkeys are only needed to get the same result as with `parted`.

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

**Optional**: Only run the next commands to create a file system for your primary/secondary partitions if you do not want to encrypt them.

1. primary partition: `mkfs.ext4 -L nixos /dev/sdXX2`
1. secondary partition: `mkfs.ext4 -L data /dev/sdXX3`

### 1.3 Encrypt primary/secondary Partitions:

> 🧐 If you use the same luks password for multiple disks, partitions and want them to autostart at boot time, you only need to enter it for one disk/partition

1. encrypt primary: `cryptsetup -y -v luksFormat --label crypto-nixos /dev/sdXX2`
1. encrypt secondary: `cryptsetup -y -v luksFormat --label crypto-data /dev/sdXX3`
1. decrypt primary: `cryptsetup open /dev/sdXX2 primary`, primary is just the name to mount the container later
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

> 🧐 If you want to make changes to your config files after the first install don't use `nixos-install` instead use `nixos-rebuild switch` or `nixos-rebuild build`

1. `nixos-install`, give a root pw at the end of install
2. reboot into your new OS 🎉

## 5. First Boot

1. add current stable package list (update version, if newer exist): `sudo nix-channel --add https://nixos.org/channels/nixos-22.11`
1. update channels: `sudo nix-channel --update`

## 6. Install Home Manager (skip this if you use Flakes)

> ⚠️ If you plan to use flakes skip these steps.

1. if you add home-manager as a NixOS Module into your configuration.nix file, add packages for home manager: `sudo nix-channel --add https://github.com/nix-community/home-manager/archive/release-22.11.tar.gz home-manager`
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
More Install Information: https://github.com/nix-community/home-manager#installation

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

4. copy config files from /etc: `cp /etc/nixos/* .`
    1. Now you no longer need to change the /etc/nixos/config files instead you change them directly in your flake
5. update: `sudo nixos-rebuild switch --flake .#` (if you only have one nixos sys configuration in flake.nix)

## 8. Home Manager with Flakes

You have two options, either you make separately or within your NixOS System Configuration (./configuration.nix)  
Option 1: Seperate

* add home-manager to your inputs/outputs in flakes.nix: `vim flakes.nix`

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
                /home/${user}/.config/home/home.nix
              # if you host your flake in a git repo
                ./home.nix
              };
            };
          };
        };
      };
    };
```

Option 2:

    
