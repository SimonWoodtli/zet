# Arch Install VM

## things you always need: text editor, terminal emulator, web browser

1. package is for graphic driver for VMs

$ `sudo pacman -S xf86-video-fbdev`

2. xorg-xinit = if you don't want to install a display manager like gdm, lightdm you can simply use the ~/.xinitrc file and put the command to launch your GUI

$ `sudo pacman -S xorg xorg-xinit`

3. base development tools (must have to build packages and more)

`sudo pacman -S base-devel`

4. to setup wallpapers use nitrogen

$ `sudo pacman -S nitrogen alacritty picom`

5. install AUR-helper (the official way to install any AUR package)

```sh
cd Repos/github.com
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

6. browser

$ `paru -S librewolf`

7. copy xinitrc from your gitlab repo to \$HOME
8. copy bash_profile from your gitlab repo to \$HOME

Related:

* [20220201002546](/20220201002546/) Arch Install
