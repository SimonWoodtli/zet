# Setup and Configure iSH: Alpine Linux iOS App

\#TODO Uninstall iSH, fresh install then set bash as default shell + git clone my dotfiles and get configs
=> see what needs changes and write a cleaner setup here.
=> check if bash is too slow if so stick with busybox
=> add only most essential things, keep it slick

## edit the login message
vi /etc/motd

## setup ssh

https://unix.stackexchange.com/questions/668927/how-to-ssh-on-alphine-linux-with-ish-on-ipad
https://github.com/ish-app/ish/wiki/Running-an-SSH-server

## Update system
apk update
apk upgrade

## Fix repos if necessary
switch to usable repos - iSH defaults often failed with EOF errors
echo https://dl-cdn.alpinelinux.org/alpine/v3.13/main > /etc/apk/repositories
echo https://dl-cdn.alpinelinux.org/alpine/v3.13/community >> /etc/apk/repositories

## Git clone fix (if not working)

apk del git
wget https://dl-cdn.alpinelinux.org/alpine/v3.11/main/x86/git-2.24.4-r0.apk
apk add ./git-2.24.4-r0.apk
git config —global pack.threads “1”
https://github.com/ish-app/ish/issues/943


## Install programs

```
apk add tmux tmux-doc vim vim-doc openssh
apk add git bat jq net-tools lynx file
apk add sed attr dialog dialog-doc bash bash-doc bash-completion grep grep-doc
apk add util-linux util-linux-doc pciutils usbutils binutils findutils readline
apk add lsof lsof-doc less less-doc  curl curl-doc
```

## Add python

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

## Setup scripts

* lynx
* dotfiles
* tmux
