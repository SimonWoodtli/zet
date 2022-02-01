# LINUX: Configure/Setup Apple Keyboard (Switch keys)

```sh
#!/usr/bin/bash

[[ ! -d $HOME/repos/github.com/free5lot ]] && mkdir -p \
  $HOME/repos/github.com/free5lot
cd $HOME/repos/github.com/free5lot
git clone https://github.com/free5lot/hid-apple-patched.git
cd $HOME/repos/github.com/hid-apple-patched
./build.sh
./install.sh
echo "options hid_apple swap_fn_f13_insert=0
options hid_apple fnmode=2
options hid_apple swap_fn_leftctrl=1
options hid_apple swap_opt_cmd=1
options hid_apple rightalt_as_rightctrl=1
options hid_apple ejectcd_as_delete=1
" | sudo tee -a /etc/modprobe.d/hid_apple.conf
sudo update-initramfs -u
```

Related:

    <https://wiki.archlinux.org/index.php/Apple_Keyboard>
    <https://github.com/free5lot/hid-apple-patched>
