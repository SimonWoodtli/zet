# Customize Gnome Desktop

Applies to Gnome 42

Settings -> Appearance => Dark

1. Disable animations: `gsettings set org.gnome.desktop.interface enable-animations false`
2. Install Gnome Tweaks via default package manager
3. Install Extension Manager `flatpak install flathub com.mattjakeman.ExtensionManager`
4. run Fedora/install-DarkMode

Open Extension Manager and Install:

* Blur my Shell
* User Themes
* Just Perfection
* Auto Move Windows
* Hide Panel Lite
* Tray Icons: Reloaded
* Allow Locked Remote Desktop
* Rounded Window Corners
* Sound Input & Output Device Chooser
* Vitals
* Privacy Quick Settings Menu
* Quick Settings Tweaker (wait for gnome 44 support)

Stuff that i find just OK:

* Desktop Cube, get background [Panorama], not sure if it makes
  Shift-super J to move window to next workspace not work (gotta test)
* Dash to Panel
* Caffeine (doesn't always work)
* Coverflow Alt-Tab
* Rounded Corners
* Show External IP
* Light/Dark Theme Switcher

Open Gnome Tweaks:

* Window Titlebars -> Activate "Maximize" and "Minimize"
* Appearance -> Legacy Applications "adw-gtk3-dark" # This will change looks of Geary

[Panorama]: <https://polyhaven.com/hdris>
