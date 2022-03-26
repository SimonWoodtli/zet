# Create Windows 11 VM with VirtualBox

## Requirements

* 8GB RAM
* 3 CPU
* 256MB Video Memory with VBoxSVGA and 3D Acceleration
* 100GB HDD


## Bypass Error: Regedit

* At first Screen : `Shift-F10` to get the command line and `regedit`

In order to bypass the min. requirement error you have to create this dir:

`HKEY_LOCAL_MACHINE\SYSTEM\Setup\Labconfig`

Inside Dir create four new DWORD (32-bit) items. Name them:

* BypassTPMCheck
* BypassCPUCheck
* BypassRAMCheck
* BypassSecureBootCheck

And give all of them a  value of "1".

## Bypass Microsoft Account

Before you start the Machine make sure the Network adapter is turned off.
When the WIFI error comes up and you can't hit 'next' `Shift-F10` and `taskmgr` 
here you should see the `Network Connection Flow` process which you need to kill.
