# Windows Debloat and Apps

## Install with all Apps needed+debloat => Create Backup

tools needed: winreducer

Go through process

1. installed Windows10 in VirtualBox (hard disk in .vhd format) -> after the installation, as soon as the setup starts CTRL + SHIFT +F3 pressed, so that the setup is interrupted and the admin is logged in -> all programs I want installed -> restart.
2. in the window that opens now I selected "Enable Out-of-Box-Experience (OOBE) likely system", check "Generalize" and select "Shutdown".
3. mount the .vhd harddisk from the VM and write this harddisk to an install.vim file with the program Dism++.
4. unpack the original Windows10 ISO and copy the install.vim into the folder "Sources". -> overwrite already existing install.vim
5. then write the folder with my "custom" install.vim again with Dism++ into an ISO.

* [Create ISO Tutorial]
* [Windows ISOs]
* [TGF Mouse Tuning Pack]
* [TGF Tuning Pack]
* [Windows-Tool]

## Apps

* install as many apps as possible with [Chocolatey]

[Chocolatey]<https://chocolatey.org/>
[Windows-Tool]<https://christitus.com/windows-tool/>
[TGF Tuning Pack]<https://github.com/MinersWin/TGF-Tuning-Pack-4.0>
[TGF Mouse Tuning Pack]<https://github.com/MinersWin/TGF-MOUSE-TUNING-PACK-2.0>
[Windows ISOs]<https://thegeekfreaks.de/downloads/windows-10-tgf-edition/>
[Create ISO Tutorial]<https://yewtu.be/watch?v=aNS9Gqxasbk>
