# Chinese Keyboard input on Android

1. Install Trime from F-droid store
1. Give Trime read/write access via android settings
1. Open Termux: 
    1. `pkg install wget`
    1. `cd /sdcard` 
    1. `mdkir rime`
    1. `cd rime`
    1. `mkdir build`
    1. `wget https://github.com/Bambooin/rimerc/releases/download/0.1.6/rimerc-trime-0.1.6.zip`
    1. `unzip rimerc-trime-0.1.6.zip`
    1. `mv trime/* build/`
1. Go to android settings and enable Trime as keyboard input
1. Open up Trime
    1. Profile: Enable "sync in background"
    1. Hit deploy



