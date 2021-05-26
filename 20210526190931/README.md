# bboost20: Day 9

Linux Beginner Boost - Day 9 (May 14, 2020)

##  Wednesday, June 14, 2020, 01:23:24PM

### Build Computer

* if you want to get into hardware and really try to understand how thing work. You should find some old piece of crap PC and run linux on it. Make sure you can break this machine in worst case. So you might trash your HDD but you can learn how to recover from that.


#### Find a Computer

***HINTS***
* Minimum CPU specs: Linux runs on practically anything super old machines too. old chromebooks (2014) are tricky don't get into that. Nowadays modern chromebooks run linux too.
* Mint XFCE for old computers is rob's recom. or Arch since its super lightweight.
* you can collect hardware by asking people if they have old stuff laying around they don't need anymore (you can make a project out of it: saying we collect old hardware put linux on it and ship it to Africa or to people who don't have computers)
* peppermintos 32BIT for old hardware (always 32bit, of course)
* 1. use mint xfce or zorin os for very old computers

1. Linux Runs on Anything
1. Look Around (No Seriously)
1. Get a Raspberry Pi (Not Arduino)
1. Be Sure You Can Break It! (go break VM's and Dockers first, because you can snap it back)


#### Create a Bootable USB Drive

***HINTS***
* if you want to do `dd` to make bootable disk and install linux always run a checkum value evaluation.
* `dd` is a great tool to mirror images of disks or to embedding encrypted packages inside of multimedia (e.g. you can hide encrypted passwordfiles inside some multimediafiles), its also good for pentesting and hacking and manipulating the files at bit level.
* What happens if you increase your bytesize to get more stuff on your disk? `dd` is stupid it won't crash or memory fail but it can't keep up. If you turn the bytesize higher up than your disk is able to write. you can tell it to do 2gb/s it will finish really fast but it actually has not written anything on it. Because the diskdrive can't keep up with the speed. If you're lucky: your diskdrive is able to buffer that much and then write it but if it can't buffer it you will lose data.
* another problem with `dd` it does not have checksume validation, so you don't even know if your disk is able to keep up. It might be doing something else. TIPP: always synch after `dd`.
* Rob does not recommend Rufus: it sucks compared to balena etcher, and it does not run on every OS like Mac or Linux. Balena flashes everything you can imagine.
* balena etcher is electron, he says this company does other great stuff (they are at the sweet spot of modern IT)

Download Balena Etcher, Grok Why
Download a Linux Install Image
Grok Live Booting
Get a USB Thumb Drive
Flash Image to Thumb Drive

#### Understand Boot Process

***HINTS***
* Your OS= a system to manage how everything is talking with each other. All the parts of a computer need to be able to talk with each other and not interrupt each other (called interrupt request). That's what an OS does. Your computer is basically a loop at lightspeed. Everything that happens has to happen in this loop. The "bus" is a way of communicating between all these devices.
* there is only one operating system (OS) in control at a time. But it can hang over the control to a next OS. (oversimplified)

**What's the BIOS? What can you do?**
1. overclock
1. check up power and heat of devices
1. turn of the numpad
1. change the function keys by default.
1. you can change the boot order: what device gets to run (boot) next
1. after BIOS finished loading it hands off all of the control to the next thing which is the bootloader (diskdrive)

**Grok BIOS**
* The BIOS is an OS that comes with your motherboard. it's not part of Mbo but its either soldered into the Mbo or comes as a chip or it's plugable into your Mbo. That is called a ROM (read only memory). So the bios is firmware.
* firmware= more firm then soft, so software can be changed easily firmware can't you need to flash it. (danger: if you loose power during this process the chip is fried, not flashable anymore). it is low level coding that goes into really tiny chips that are responsible to getting you to the next thing.
* MAC BIOS is only BIOS with IP-stack. In other words it can download and communicate with the internet at BIOS level. That's why it is easy to reinstall a MAC OS. so you can completely mess up your OS you will always be able to recover/reinstall it because this option comes from BIOS which runs before the rest. (correction: most modern BIOS have IP
-stack ala PXE Boot)
**Grok Disk Partitions**
* disks can be separated into partitions as far as the BIOS is concerned those partitions are kind of like separate devices.
* if partition has not been set to boot it means that partition can not take over control after something else.
* disk partitions: a lot of time when you deal with partitions you actually have to remove you them explicitly from an old hard drive, when you go trough an OS install process. Why? Because it won't free the disk space up until you remove the partition. (be careful to do that. (WIN and MAC both have a second recovery partition)
* disk encryption does not slow down performance
**Grok Boot Order**
* You can learn  about bootprocess:  by experimenting with old machines and trying to brick one and recover the system.
* The bootprocess takes time to learn but is worth to understand on a low level. Learn by messing with it.
* if you turn on computer: windows does not run nor does linux the very first OS that runs is the BIOS. And BIOS goes ok which one is my HDD (correction first comes Bootloader, ten BL. sends it to HDD)? Where do I send control to next? And the BIOS sends control to the boot partition of your HDD. And the boot partition of your HDD says ok now who gets control? And it gives control to one of your other disk partition that has han OS on it. And then that starts up and you get to your login screen. (sometimes the bootpartition can also be you main partition, not always) If anything goes wrong with this process: you end up with a flashing cursor and a black screen. Or a hard drive error cant find no hard drive. If your BIOS is fried you don't see anything because it is responsible to send stuff to your screen at the beginning.
**Grok Boot Loaders**
* GRUB is linux bootloader
* turn off UEFI: If you want UEFI why? e.g. Linux Mint Cinnemon won't even load the NVIDIA proprietary drivers unless UEFI runs properly (secure boot on)
* power management can be less effective with linux.
* in the old days setting screen resolution/refresh rate in linux could if done wrong destroy your display. (linux is that powerful, so that it can damage your hardware if you run it wrong)

**Grok UEFI**
* UEFI why care? The UEFI addition has made the installation of Linux really hard. Legacy BIOS don't have to deal with UEFI. UEFI was built by windows and intel to create a way to authorize secure hardware. In other words prevent Linux from being installed. It was a to protect you from one thing hackers can do: If you plugin a USBstick reboot it and bootup the stick you own this computer. It is very easy to do that if it's running in legacy mode. Because why? You bypassed all of the security of the system. Counter that: encrypt your HDD.
* UEFI was originally designed to protect us.
* PopOS installation is super easy. And powerful under the hood. During installation process it completely deals with the UEFI issue without bothering the user with it. Normally you would need to turn off Secure Boot in UEFI (also called enable LEGACY-Mode). With pOp OS you don't. It detects to whatever the BIOS is set to and it figures it out. Another thing they did: they made stuff work that is proprietary like NVIDIA out of the box.
* With Linux you have now: 1. BIOS that boots 2. UEFI Bootloader that boots 3. and that boots GRUB (another Bootloader) 4. and GRUB boots your OS.
**Grok Grub**
**Grok Init Levels**
**Grok Danger of Powering Off**

Research Your Computer Type
Identify Setup Key Sequence
Change Your Boot Order to USB First
**Dual Booting**
Recommendations are for real beginners:
* Dual Booting is controversial, don't do it, many destroy their systems
* why do you want it? because they wanna run on hardware (it's faster) and may still need windows
* most people want to keep the windows machine intact and the risk of screwing it up is high






Grok Pros and Cons
Consider Mint or Easy Dual Boot

**Make It Safe**
* why do you care? you can be fired for doing this wrong
* if you install linux on a corporate computer you must follow corporate policies or you may get fired over it. they are very strict on this because they have their own distros. The number one source of IP espionage is compromised laptops.
* setting up secure boot with password: if you forget it your Mbo is bricked. There are no backdoors to get back in. (boot password is a bad idea, because the risk of loosing the password is too high) unless your motherboard has a bios password reset jumper
* encrypt your OS HDD is enough but really important. You can also set a nuke password to cut off stuff if you want and no one gets it.
* modern disk encryption is so fast so no performance loss there. Because there is a buffer that sits between what gets written to the disk and what doesn't and that buffer is actually the weak spot in disk encryption. But that buffer is able to handle it and you won't see any degredation in performance. Unless you put a database on your computer, that would be a different story.
* encryption is not necessary on VM unless its bridged
* If you run a server you should run those commands as often as possible (that's why it's also better to run your website from a provider like Netlify because they take care of that)
* If you agree to put a server on the internet you implicitly agree to keep it up2date and if you don't you will be owned.
* Get a alpha wifi dongle because  some of them can be put in promiscuous mode (alfa can), it will give you wifi access. Its nice to have a working dongle if your hardware does not. Broadcom is the devil of hardware manufactures in the linuxword.
* a wifi dongle on monitor mode is as if you have a package sniffer on every single line between the computer and that line. that is awesome and terrifying at the same time. because that means you can watch every single package on that network. This makes it super easy to hack wifi with aircrack software.
* how to be save from a package sniffer? use encrypted protocols only: https ssh (no http no telnet)
* wifi speed is limited to the slowest thing on your computer network and that brings the speed down for every device, range and rain also decrease speed.
* hackers only use ethernet and wifi if they own others peoples wifi.

**you wanna check for open listeners**
`nmap localhost` now you can see all the things that are running. If any of those things is not setup from you it might be compromised.


1. first update and upgrade your system
1.

Update and Upgrade Immediately
Consider How and Where Will Be Used
Consider Disk Encryption
Get a Wifi Dongle Just In Case
Consider Ethernet Over Wifi
Create a User with Sudo
Check for Open Listeners

#### Consider Backup Strategies

* put your code and all the notes you care about into the cloud. encrypt it!
* make sure your computer becomes a temporary device that could be thrown away at any moment. Because you will destroy your computer at some point.
* leverage the cloud as good as possible and also leverage automation. You can install stuff totally automatically it saves lots of time!

----

### General Notes

* electron= nodes.js + over websockets and chromium as a bundle
* USB is an eprom: if you plugin a usbstick it also has it's own firmware that then allows it to do other things with the rest of the usbstick
* these days you don't need that many partitions anymore. In the old days they said you need one for HOME one for SWAP one for BOOT a this and this.
* MBR = master boot record, not a beginner topic but you should learn eventually. It's the first bit of data right at the beginning of the boot partition that says what else to do after that.
* if computer freezes it is often because it is running out of memory (RAM) -> happesn to VM too

#### Common Practice

1. Live booting is nice for SecOps

#### Tips

1. If you install something or run a new OS or experiment make notes! write down each tiny step so you can reproduce your experiment.
1. Knowledge management is essential for autodidact.
1. Take your notes in basic pandoc markdown
1. install watch sensors (CLI tool to see temp. and stuff)
1. Rubber Ducks is a live OS that also runs something. It can own a Windows system and copy all the files on the usbstick.
1. don't use swap for SSD cards! it burns out your drive
1. don't `cat` into a pipe or loophole it may screw up your variables
1. try to find a makerspace: if you can't find one at your place make your own lab to do fun hardware related projects
