# Dotfile Manager: chezmoi

For a cloud-native OS approach having a separate CLI workspace alone is not enough. The dotfiles also need to be able to get rewritten and updated when the workspace image gets rebuild.

## Advantages

Currently I use a second local repo that has more sensitive information or configuration in it. Using `chezmoi` means no longer having to separate the two. Also having to manage `setup` scripts to symlink configurations for different programs becomes a thing of the past.

* Password manager support
* Full file encryption with `gpg` or `age`: Encrypts secret files so you can keep them in your dotfiles repo. 
* Manage your dotfiles across multiple diverse machines, securely. Put Configurations in different Folders for different operating systems (Linux|Mac|Windows).
* Templates allow for custom profiles for machines that don't need all configuration or different versions of the same configuration file.
* Go binary, no scripting language dependency for running it
* Creates actual files and copies them to right location (symlinking is not the best approach)
* Still utilizes the git repo approach

## Installation

* Mindows: `choco install chezmoi`
* Mac: `nix-env -i chezmoi`
* Linux: 
    * Nix: `nix-env -i chezmoi` 
    * Alpine: `apk add chezmoi`
    * Termux: `pkg install chezmoi`

Any other distro: `sh -c "$(curl -fsLS get.chezmoi.io)"`   
The shell script install created a /bin folder in `pwd` and put chezmoi in there. I just moved it to ~/.local/bin

## First Setup

What chezmoi does: It looks at what kind of file and where the file is stored and both information are being used when it copies the file into .local/share/chezmoi. e.g a dot file gets named `dot_bashrc`. It will recreate the folder structure, so `feh` which stores config files in ~/.config/feh config is named `dot_config/feh/keys`.

> 🧐 initializing creates a new local git repo in `/home/username/.local/share/chezmoi` with the remote that points to your github dotfiles.git

> ⚠️ If your current dotfile system has a setup script to symlink all the dotfiles repo files to the right place don't use `chezmoi add ~/.bashrc`. This would just add the symlink to `chezmoi`. Instead use `chezmoi add --follow ~/.bashrc`


1. install chezmoi
1. create new empty dotfiles repo on github: `gcr` (I don't want all the bloat
   that accumulated over time in my dotfiles-old)
1. initialize chezmoi with ssh (prefer): `chezmoi init git@github.com:SimonWoodtli/dotfiles.git`
      1. initialize chezmoi with https: `chezmoi init https://github.com/SimonWoodtli/dotfiles.git`

1. configure chezmoi: ??
1. adding symlinks: `chezmoi add -f ~/.bashrc`
   Otherwise you would need `rm ~/.bashrc && cp ?/dotfiles-old/bashrc ~/.bashrc
   && cd && chezmoi add .bashrc` (if you migrate from old symlink system)
1. adding files to be managed as templates: `chezmoi add --autotemplate ~/.gitconfig`

https://jerrynsh.com/content/images/2022/02/Getting-Started-With-Chezmoi-1.png
`chezmoi add  -T --autotemplate ~/.gitconfig`

https://fedoramagazine.org/take-back-your-dotfiles-with-chezmoi/
https://dev.to/jmc265/using-bitwarden-and-chezmoi-to-manage-ssh-keys-5hfm

`chezmoi add --encrypt  foo`
`chezmoi add --template foo`


## Setup any New Machine

Not sure if `chezmoi init --apply SimonWoodtli` will use https git remote or ssh (need to test it)

1. Install and init: `sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply $GITHUB_USERNAME`

1. Install:
1. Init: `chezmoi init --apply SimonWoodtli`




Related:

* <https://www.jacobbolda.com/chezmoi-dotfile-management>
