# Gnome Custom Keybindings with Scripts

If you have UNIX commands that you want exec a custom command and you want to be able to run it with a hotkey you have three options.

1. inputrc
2. gsettings
3. GUI 'Settings'

You also need to wrap your commands into a script. As the unix commands require the shell.

> 🧐 gsettings and the GUI 'Settings' both change configurations in the dconf database so they are essentially the same. One CLI and and GUI solution.

## What to use when?

> 🧐 If you use inputrc you keybindings will only work while the active window is a terminal. This is IMO the best option to create a keybinding that exec a shell script where no GUI is involved. There is the option to get terminal output with a gsettings shortcut. You start it with a command like `gnome-terminal -- <scriptname|command`. But that will create a terminal and than run the script in this terminal. So it's probably not what you want. Obviously you also can't just set unix commands directly into the command field from your keyboard settings.

Inputrc: Best choice if it's a script where the final output is a terminal
GUI 'Settings': Best choice if it's a script where the final output is a GUI
gsettings: same as GUI 'Settings'

examples:

* output shell: echo 'hello world' => solution inputrc
* output gui: echo ‘hello world’ | wofi -d => solution gsettings

## How to use

> 🧐 If you use gsettings|GUI 'Settings' make sure that you put your script with your custom commands inside /bin. The shortcuts only look for globally installed programs.

1. Use inputrc: `Control-o: <scriptname|command>` put this line into ~/.inputrc
1. Use GUI: 
    1. `sudo touch /bin/foo` and then write your custom command
    1. 'Settings' 'Keyboard' 'View and Customize Shortcuts' 'Custom Shortcuts' '+'
    1. Name: my foo command Command: foo Shortcut: Ctrl-o
1. Use gsettings:
    1. `sudo touch /bin/foo` and then write your custom command
    1. checkout all the [required gsettings commands] (interesting if you want to create a script to create keybindings)

### Wayland Tools for Script

* dotool to write output
* wofi to select output

### Xorg Tools for Script

* xdotool to write output
* dmenu to select output

[required gsettings commands]: <https://www.suse.com/support/kb/doc/?id=000019319>

Tags:

    #linux #gnome #keybinding #hotkey #shortcut #tutorial #scripting #wayland #xorg #gui #shell
