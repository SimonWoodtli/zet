# Chinese Keyboard Input on Android: Trime IME

> ⚠ Leave a Settings Window/App open so you can disable Trime, it
is buggy as fuck and worst case it would be in a state of constant crash
which without settings open would be annoying to disable trim keyboard
input and you are in a crash loop!

> ⚠ Whatever you do do not setup Trime as default keyboard it is too buggy
and might end in a bug/crash loop

1. Install Trime from F-droid store
1. Open Termux: 
    1. `pkg install wget`
    1. `cd /sdcard` 
    1. `mdkir rime`
    1. `cd rime`
    1. `wget https://github.com/Bambooin/rimerc/releases/download/0.1.6/rimerc-trime-0.1.6.zip`
    1. `unzip rimerc-trime-0.1.6.zip`
1. Open up Trime: give read/write access and enable trime keyboard input (first time launch)
    1. Profile: Enable "sync in background"
    1. Profile: Set  User and Shared Directory manually: `/sdcard/rime/`
    1. Theme & Color: Select Colors (this should populate your default
       config files into /sdcard/rime/, maybe you need to do this a couple of times)
1. Termux:
    1. Check if new files have been populated such as build/ opencc/
       trime.yaml installation.yaml etc.
    1. If not keep fiddeling around in Trime App settings to trigger
       this
    1. Once you have them: `cd /sdcard/rime/`
    1. `cp -R trime/* build/ .`
    1. `cp -R trime/* .`
1. Trime: Hit deploy
    1. Theme & Color: Enable "Auto change dark/light color scheme" then you can change color to 'Steam' (darkmode)
    1. Hit deploy again
    1. Theme & Color: Disable "Auto change dark/light color scheme" to activate darkmode
    1. Hit deploy again


Tags:
        #android #chinese #keyboard
