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
    1. Check if enabled: `cat /etc/apt/apt.conf.d/20auto-upgrades`
    1. Dry run: `sudo unattended-upgrade --dry-run --debug`
1. Install some goodies: `apt install git curl vim fzf tmux`
1. Get dotfiles and apply them:

```
mkdir -p Repos/github.com/SimonWoodtli/
git clone -C ~/Repos/github.com/SimonWoodtli https://github.com/SimonWoodtli/dotfiles.git
chezmoi -S ~/Repos/github.com/SimonWoodtli/dotfiles init --apply
```

8. TODO: research - more steps?
9. Reboot server

Related:

* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104134254](/20240104134254/) Server Security and Setup: Make it cosy
* [20240104124550](/20240104124550/) Server Security: Additional SSH Hardening
* [20240104130222](/20240104130222/) Server Security: Config ufw Firewall

Tags:

      #linux #server #dotfiles #configs #networking #updates
