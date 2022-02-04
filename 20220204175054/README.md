# Raspian OS: Aero Snap feature (split windows like WIN10)

1. get dependencies:
`sudo apt install -y build-essential libx11-dev libgtk-3-dev wmctrl git`

2. clone repo: `git clone https://github.com/lawl/opensnap.git`
3. install:

```sh
cd opensnap
make
sudo make install
mkdir -p ~/.config/opensnap
cp /etc/opensnap/* ~/.config/opensnap/
```

3. autostart software: `sudo vi /etc/xdg/lxsession/LXDE-pi/autostart`
Content:
@opensnap -d

## Usage

Alternative: CTRL-ALT  ARROW KEYS to split windows

Tags:

    #linux #raspian #windowManagement
