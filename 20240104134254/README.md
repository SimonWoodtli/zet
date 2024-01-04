## Server Security and Setup: Make it cosy

> 🧐 Auto security patch updates are convenient. In many cases the benefits
> outweight the disadvantages. However it's still better to only use that on
> your personal projects.

1. change welcome msg with some ASCII Art :) `vi /etc/motd`
1. change hostname: `hostnamectl set-hostname my-server-name` then update the
   /etc/hosts file: add under localhost entry `127.0.1.1 my-server-name`
1. consider changing the NTP time server
1. check time zone: `timedatectl` and maybe you want/need change to UTC:
   `timedatectl set-timezone Etc/UTC` (not always a good idea)
1. install auto upgrades: `sudo apt install unattended-upgrades`
    1. enable auto upgrades for security patches: `sudo dpkg-reconfigure --priority-low unattended-upgrades`
    1. check if enabled: `cat /etc/apt/apt.conf.d/20auto-upgrades`
    1. dry run: `sudo unattended-upgrade --dry-run --debug`
1. install some goodies: `apt install git curl vim fzf tmux`
1. Get dotfiles and apply them:

```
mkdir -p Repos/github.com/SimonWoodtli/
git clone -C ~/Repos/github.com/SimonWoodtli https://github.com/SimonWoodtli/dotfiles.git
chezmoi -S ~/Repos/github.com/SimonWoodtli/dotfiles init --apply
```

8. TODO: research - more steps?
9. reboot server

Related:

* TODO add zet serverinit, ssh-hardening, firewall

Tags:

      #linux #server #dotfiles #configs #networking #updates
