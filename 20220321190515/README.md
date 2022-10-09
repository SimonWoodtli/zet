# Use USB devices within a VirtualBox VM 💻

1. Download the [VirtualBox Software] and install it
1. Download the [Extensions] and install it
1. Add your current user to the vboxusers group: `sudo gpasswd -a $USER vboxusers`
1. Reboot system
1. Make sure your VM is shutdown and go to Settings to add your USB device

> Hint: On Windows Host Machines you also need to install [VBoxWindowsAdditions.exe] 
which can be done while the VM is running under Devices "Insert Guests Additions Image"


Related:

* https://docs.oracle.com/cd/E26217_01/E26796/html/qs-guest-additions.html

[VirtualBox Software]: <https://www.virtualbox.org/wiki/Linux_Downloads>
[Extensions]: <https://download.virtualbox.org/virtualbox/6.1.32/Oracle_VM_VirtualBox_Extension_Pack-6.1.32.vbox-extpack>
[VBoxWindowsAdditions.exe]: <https://www.oracle.com/virtualization/technologies/vm/downloads/virtualbox-downloads.html>
