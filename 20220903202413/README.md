# Setup and Configure iSH: Alpine Linux iOS App

=> add only most essential things, keep it slick

Note: Vim is slow to open, so better make sure to have fzf, nerdtree or something alike installed so you don't have to quit out of vim often.

Common APK commands: https://www.cyberciti.biz/faq/10-alpine-linux-apk-command-examples/?utm_source=Linux_Unix_Command&utm_medium=faq&utm_campaign=nixcmd

## Overview

1. Download and install the official Alpine Mini Root [Filesystem] - x86:
2. Make bash default shell
3. Install software
4. Git clone dotfiles, zet => setups configs
5. Make iSh pretty
6. SSH

## 1. Download Filesystem and Update

In order for iSh to be published on the appstore they had to use and maintain their own Repos. However It's easy to get the official Alpine Repos working on iSH and with it way more packages can be installed.

1. Download Mini Root [Filesystem] - x86 (weird iSh is x86 why not arm?)
2. Go to iSh settings -> filesystem and select new downloaded FS
3. Update software:

```
apk update
apk upgrade
```

4. If you don't want to install the Mini Root FS. You should at least use the official Alpine Linux Repos. iSh ships with its own limited Repos to bypass Apple and be available on the store.


* If you do decide to use Alpines Repos make sure to add the latest version currently 3.16 (10.2022)

```
echo https://dl-cdn.alpinelinux.org/alpine/v3.16/main > /etc/apk/repositories
echo https://dl-cdn.alpinelinux.org/alpine/v3.16/community >> /etc/apk/repositories
sed -i -e '/http:\/\/apk.ish.app/d' /etc/apk/repositories
```

* Or use the bleeding edge packages: `vi /etc/apk/repositories`

```
http://dl-cdn.alpinelinux.org/alpine/edge/main
http://dl-cdn.alpinelinux.org/alpine/edge/community
```

## 2. Make Bash default shell

To use usermod/groupmod you need the shadow package on Alpine Linux.


1. `apk add bash shadow`
2. `usermod --shell /bin/bash root`
3. to confirm: `echo $SHELL` and `grep root /etc/passwd`

## 3. Install software

Try to keep the installed software to an absolute minimum

1. Install essentials

  * add man-pages: `apk add mandoc man-pages mandoc-apropos less less-doc`

```
apk add tmux tmux-doc vim vim-doc openssh openssh-doc openrc
apk add git git-doc bat bat-doc jq jq-doc net-tools lynx lynx-doc file
apk add sed sed-doc attr dialog dialog-doc bash bash-doc bash-completion grep grep-doc
apk add util-linux util-linux-doc pciutils usbutils binutils findutils readline
apk add lsof lsof-doc curl curl-doc
```

### Issues

* add git RESOLVE (fix): `git config --global pack.threads 1`
* bat and nodejs both through an illegal instructions error. (maybe fixed with next update)

### Install golang

`apk add go go-doc`

### Install nodejs

Currently NodeJS is not working but the next update should fix that

`apk add nodejs nodejs-doc npm`

### Install python

```
apk add python3 python3-doc
python3 -m ensurepip
pip3 install virtualenv
```

start virtualenv for python:

mkdir foo
cd foo
virtualenv venv
source venv/bin/activate

## 4. Git clone & configs

1. edit the login message: `vi /etc/motd`
2. If git is not working after adding pack.threads 1 to gitconfig get an older version of git.
https://github.com/ish-app/ish/issues/943

```
apk del git
wget https://dl-cdn.alpinelinux.org/alpine/v3.11/main/x86/git-2.24.4-r0.apk
apk add ./git-2.24.4-r0.apk
git config --global pack.threads 1
```

3. git clone dotfiles & zet repo then: `git config --global user.name SimonWoodtli`
4. install github scripts

* `mdkir -p $HOME/.local/bin`
* run install-youtube-dl script
* run install-tldr script (currently not working)

### Run Setup scripts

* lynx
* dotfiles
* tmux
* vim

## 5. Appearance

1. wait for new update to customize colorscheme
2. download [ubuntu nerd font]
3. install iFont App and install nerd font ubuntu/hack mono through it
4. go to ish settings appearance and change font

## 6. SSH

Don't add user just keep root. -> It already runs in a safe isolated app on iOS.
Alpine uses openrc as its init program, which will manage the startup of your sshd service for each new iSH session.

* debug: `ssh root@iOSip -vvv`

* enable 'local network' acces from within iOS settings for iSh
* add root password `passwd`
* install software: `apk add openssh openrc`
* create host keys: `sshkey gen` (script in dotfiles, or `ssh-keygen -A`)
* allow root login: `echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config`
* add service: `rc-update add /usr/sbin/sshd default`
* run service: `rc-service sshd start`
* check status: `rc-status default`
* to make it more secure only allow key-based authentication and disable password login.

### Login SSH

As Server:

Login to your iOS device from another computer: `ssh root@iOSip`

1. Make sure Display is on and App is open. (It only works if display is on and app is open, it does not run in the background either)
2. I recommend to turn off Auto-Lock so the screen stays on all the time.
3. Sometimes connections gets refused run. To fix it: `rc-status default`

As Client:

To use iSh as a client there is nothing to do. Except if you have a firewall or a router with blocked ports.

Links:

https://unix.stackexchange.com/questions/668927/how-to-ssh-on-alphine-linux-with-ish-on-ipad
https://github.com/ish-app/ish/wiki/Running-an-SSH-server

[Filesystem]: <https://alpinelinux.org/downloads/>
[ubuntu nerd font]: <https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Hack/Regular/complete>
