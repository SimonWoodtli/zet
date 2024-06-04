# Linux Recovery: System rescue

## Using Rescue/Recovery Media Image

The rescue image will ask a number of questions upon starting. One of these is
whether or not to mount your filesystems (if it can).

If so, they are mounted at somewhere, usually at /mnt/sysimage. You can move to
that directory to get to your files or you can change into that environment
with the following command:

```
sudo chroot /mnt/sysimage
```

For a network-based rescue you may also be asked to mount /mnt/source.

You may install software packages from inside the chroot-ed environment. You
may also be able to install them from outside the chroot-ed environment. For
example, on an RPM-based system, by using the --root option to specify the
location of the root directory:

```
sudo rpm -ivh --force --root=/mnt/sysimage /mnt/source/Packages/vsftpd-2*.rpm
```

## System Rescue and Recovery

Sooner or later, a system is likely to undergo a significant failure, such as
failing to boot properly, to mount filesystems, initiate a desktop display,
etc. System Rescue media in the form of optical disks or portable USB drives
can be used to fix the situation. Booting into either emergency or single user
mode can enable using the full suite of Linux tools to repair the system back
to normal function.

## Emergency Boot Media

Emergency boot media are useful when your system won't boot due to some issue
such as missing, misconfigured, or corrupted files or a misconfigured service.

Rescue media may also be useful if the root password is somehow lost or
scrambled and needs to be reset.

Most Linux distributions permit the install media (CD, DVD, USB) and/or Live
media to serve a double purpose as a rescue disk, which is very convenient.
There are also special-purpose rescue disks available.

Live media (in any format) provide a complete and bootable operating system
which runs in memory, rather than loading from disk. Users can experience and
evaluate an operating system and/or Linux distribution without actually
installing it, or making any changes to the existing operating system on the
computer.

Live removable media are unique in that they can run on a computer lacking
secondary storage, such as a hard disk drive, or with a corrupted hard disk
drive or file system, allowing users to rescue data.

## Using Rescue Media

Whether you are using Live, install or rescue media, the procedures for
entering into a special operating system for rescue and recovery are the same,
and as we have pointed out, one medium serves all three purposes.

The rescue/recovery mode can be accessed from an option on the boot menu when
the system starts from the removable media. In many cases, you may have to type
rescue on a line like:

boot: Linux rescue

We can't tell you all the possibilities, as each distribution has somewhat
different, but easy to ascertain procedures.

Next, you can expect to be asked some questions, such as which language to
continue in, as well as make some distribution-dependent choices. Then, you
will be prompted to select where a valid rescue image is located: CD/DVD, Hard
Drive, NFS, FTP, or HTTP.

The selected location must contain a valid installation tree, and the
installation tree must be for the same Linux version as the rescue disk, and if
you are using removable media, the installation tree must be the same as the
one from which the media was created. If you are using a boot.iso image
downloaded from the vendor, then you will also need a network-based install
tree.

Additional questions are asked about mounting your filesystems. If they can be
found, they are mounted under /mnt/sysimage. You will then be given a shell
prompt and access to various utilities to make the appropriate fixes to your
system.

`chroot` can be used to better access your root (/) filesystem.

## Rescue USB Key

Many distributions provide a boot.iso image file for download (the name may
differ). You can use dd to place this on a USB key drive as in this command:

```
dd if=boot.iso of=/dev/sdX
```

assuming your system recognizes the removable drive as /dev/sdX. Be aware this
will obliterate the previously existing contents on the drive!

Assuming your system has the capability of booting from USB media and the BIOS
is configured for it, you can then boot from this USB drive. It will then
function in the same fashion as a rescue CD or DVD. However, note that the
install tree will not be present on the USB drive; therefore, this method
requires a network-based install tree if one is needed.

Helpful utilities such as livecd-tools and liveusb-creator allow specification
of either a local drive or the Internet as the location for obtaining an
install image, and then do all the hard work of constructing a bootable image
and burning it on the removable drive. This is extremely convenient and works
for virtually all Linux distributions.

## Emergency Mode

In emergency mode you are booted into the most minimal environment possible.
The root filesystem is mounted read-only, no init scripts are run and almost
nothing is set up.

The main advantage of emergency mode over single-user mode (to be described
next) is that if init is corrupted or not working, you can still mount
filesystems to recover data that might be lost during re-installation.

To enter emergency mode, you need to select an entry from the GRUB boot menu
and then hit e for edit. Then add the word emergency to the kernel command line
before telling the system to boot. You will be asked for the root password
before getting a shell prompt.

## Single User Mode

If your system boots, but does not allow you to log in when it has completed
booting, try single user mode. In single user mode:

* init is started
* Services are not started
* Network is not activated
* All possible filesystems are mounted
* root access is granted without a password
* A system maintenance command line shell is launched

In this mode, your system boots to runlevel 1 (in SysVinit language). Because
single user mode automatically tries to mount your filesystem, you cannot use
it when your root filesystem cannot be mounted successfully, or if the init
configuration is corrupted.

To boot into single user mode, you use the same method as described for
emergency mode with one exception, replace the keyword emergency with the
keyword single.

Tags:

    #linux #sysadmin #rescue #recovery
