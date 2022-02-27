# Write emojis in your terminal

## Keyboard input to select from written word

1. Install the keyboard input:

```bash
sudo apt install ibus gir1.2-ibus-1.0
git clone https://github.com/salty-horse/ibus-uniemoji.git
cd ibus-uniemoji
sudo make install
restart ibus
```

2. Now you just have to add the keyboard input (other) called uniemoji

## GUI to select from all emojis with mouse

1. download the latest [Appimage]
1. integrate it into your system
1. in settings change what happens after clicking from "typing" to "copy"
1. launch with ctrl-super-space or ctrl-super-f (search)
1. launch at startup: `/home/sero/Applications/./emoji-keyboard-3.1.1_f891f234d97b1744bce881ace07c4b65.AppImage %U`

[Appimage]: <https://github.com/OzymandiasTheGreat/emoji-keyboard/releases>
