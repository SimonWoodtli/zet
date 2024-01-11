# Server Security and Setup: Make it cosy

> 🧐 Auto security patch updates are convenient. In many cases the benefits
> outweigh the downsides. However it's probably better to only use that on
> your personal projects.

1. Change welcome msg with some ASCII Art :) `vi /etc/motd`
1. Change hostname: `hostnamectl set-hostname my-server-name` then update the
   /etc/hosts file: add under localhost entry `127.0.1.1 my-server-name`
1. Consider changing the NTP time server
1. Check time zone: `timedatectl` and maybe you want/need change to UTC:
   `timedatectl set-timezone Etc/UTC` (not always a good idea)
1. Install auto upgrades: `sudo apt install unattended-upgrades`
    1. Enable auto upgrades for security patches: `sudo dpkg-reconfigure --priority-low unattended-upgrades`
    1. Depending on OS you need to edit `vi /etc/apt/apt.conf.d/50unattended-upgrades`
            1. On Ubuntu you are good
            2. On Debian you need to uncomment the update and proposed updates in `Unattended-Upgrade::Allowed-Origins { ... }`
    1. Check if enabled: `cat /etc/apt/apt.conf.d/20auto-upgrades` and `systemctl status unattended-upgrades.service`
    1. Dry run: `sudo unattended-upgrade --dry-run --debug`
1. Install some goodies: `apt install git curl vim fzf tmux jq`
1. Clone dotfiles, install `asdf` and apply dotfiles with `chezmoi`:

```
mkdir -p Repos/github.com/SimonWoodtli/
git clone -C ~/Repos/github.com/SimonWoodtli https://github.com/SimonWoodtli/dotfiles.git
~/Repos/github.com/SimonWoodtli/dotfiles/install/install-asdf
source "$HOME/.asdf/asdf.sh"
source "$HOME/.asdf/completions/asdf.bash"
asdf plugin add chezmoi
asdf install chezmoi latest
echo "chezmoi 2.43.0" > ~/.tool-versions
chezmoi -S ~/Repos/github.com/SimonWoodtli/dotfiles init --apply
```

8. Install golang:

```
asdf plugin add golang
asdf install golang latest
```

9. add cronjob to auto pull dotfiles:

```
```

8. Reboot server

Related:

* https://www.linuxcapable.com/how-to-configure-unattended-upgrades-on-ubuntu-linux/
* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104134254](/20240104134254/) Server Security and Setup: Make it cosy
* [20240104124550](/20240104124550/) Server Security: Additional SSH Hardening
* [20240104130222](/20240104130222/) Server Security: Config ufw Firewall

Tags:

      #linux #server #dotfiles #configs #networking #updates
