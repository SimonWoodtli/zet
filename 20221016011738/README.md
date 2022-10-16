# Connect/Mount iPhone/iPad on Linux

## Fedora

1. Install software: `sudo dnf install ifuse libimobiledevice-utils libimobiledevice`
2. unlock ipad/iphone with pin then run `idevicepair pair` touch trust and run `idevicepair pair` again.
3. allow multiple connections: `usbmuxd -f -v`
4. create a folder within your ~ to mount iphone/ipad
5. mount phone/ipad: `ifuse ~/foo`

## Debian

Should be the same need to test

## Arch

Should be the same need to test

Related:

* <https://www.maketecheasier.com/easily-mount-your-iphone-as-an-external-drive-in-ubuntu/>

Tags:

    #iphone #ipad #linux #mount #dataTransfer #pictures #movies
