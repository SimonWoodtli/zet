# Libreboot Thinkpad T480S

TODO: write a howto update libreboot zet

Hardware changes:

* I didn't want to buy a B+M keyed nvme ssd as they are pretty odd and I don't know any other project I could ever reuse that drive on. So I got an adapter to get the  2242 length b+m keyed wwan slot to a 2230 length nvme ssd. Just search for 'b+m adapter 2242 to 2230k'
* I used a 1TB corsair MP600 mini for the wwan slot as my OS drive
* I used a 4TB NV3 Kingston as my 2ndHDD drive for data
* I upgraded the wifi to:

## Some Notes

* You can do updates/upgrading libreboot via host OS [internally] on a running
  linux OS. No need to do the cumbersome hardware/jumpwire (externally) on the
  chip directly.
* From my understanding whatever revision you are trying to flash you also need
  to get the most up to date software/revision that matches the firmware
  revision onto your [raspberry pi pico][pico].
* Get the latest libreboot firmware here: <https://mirrors.mit.edu/libreboot/stable/XXX/roms/>
* Get the latest pico firmware here: <https://mirrors.mit.edu/libreboot/stable/XXX/roms/> (look for xxx_serprog_pico.tar.xz)

## External Libreboot Flash: Instructions

I didn't do a lot of edits on this instructions they are from the YT-Guide
that dude pretty much nailed it and I just edited a little bit.

IMPORTANT: One thing to note is that there are two chips that needed to be
flashed separately with the raspberry pico. The thunderbolt chip with the
firmware fix and the bios chip for libreboot itself.

```
Install Git
sudo apt install git

Configure Git
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

Build libreboot
git clone https://codeberg.org/libreboot/lbmk
cd lbmk
export XBMK_THREADS=4
./mk dependencies mint
./mk -b coreboot t480_vfsp_16mb

Build flashprog
./mk -b flashprog

Build pico-serprog
wget https://mirrors.mit.edu/libreboot/stable/20241206/roms/libreboot-20241206rev9_serprog_pico.tar.xz
tar -xvf libreboot-20241206rev9_serprog_pico.tar.xz

1. Push btn on pico and keep it pushed down whilst plugging the usb cable to your laptop. It should now show up as a storage device on your laptop.
2. Go to ~/T480S/lbmk/bin/serprog_pico (look for the pico version you have and copy that onto the pico), this will flash the pico which in turn will make it able to flash libreboot onto a bios chip.
3. Find out on which tty number pico runs: (if nothing else plugged in it should be ttyACM0): `dmseg`

Inject vendor files
wget https://mirrors.mit.edu/libreboot/stable/20241206/roms/libreboot-20241206rev8_t480_vfsp_16mb.tar.xz
./mk inject libreboot-20241206rev8_t480_vfsp_16mb.tar.xz (./mk inject libreboot-RELEASE_targetname.tar.xz)

Flash Thunderbolt
flashprog -p serprog:dev=/dev/ttyACM0 -r ../../../t480tb1.bin
flashprog -p serprog:dev=/dev/ttyACM0 -r ../../../t480tb2.bin
sha512sum ../../../t480tb*.bin (MAKE SURE BOTH HASHES MATCH)
flashprog -p serprog:dev=/dev/ttyACM0 -E
dd if=/dev/zero of=null.bin bs=1M count=1
flashprog -p serprog:dev=/dev/ttyACM0 -w null.bin
NOW, REMOVE the clip. Button it back up and turn the T480 on, with both the battery and the charger connected, plugging in the battery first.
flashprog -p serprog:dev=/dev/ttyACM0 -w ../../vendorfiles/t480/tb.bin
NOW, REMOVE the clip. Button it back up and turn the T480 on, with both the battery and the charger connected, plugging in the battery first.

Flash libreboot external with pico:
IMPORTANT: on my t480s flashprog required to specify the bios chip with the -c flag: sudo ./flashprog -p serprog:dev=/dev/ttyACM0 -c 'MX25L12833F/MX25L12835F/MX25L12845E/MX25L12865E/MX25L12873F' -r ./../../t480bios1.rom (this may be different depending on the actual chip, you can literally read the chipname on the chip itself although it is very small written on top of the chip)
cd ../..
tar -xvf libreboot-20241206rev8_t480_vfsp_16mb.tar.xz
cd elf/flashprog
sudo ./flashprog -p serprog:dev=/dev/ttyACM0 -r ../../../t480bios1.rom
sudo ./flashprog -p serprog:dev=/dev/ttyACM0 -r ../../../t480bios2.rom
sha512sum ../../../t480bios*.rom (MAKE SURE BOTH HASHES MATCH)
sudo ./flashprog -p serprog:dev=/dev/ttyACM0 -w ../../bin/t480_vfsp_16mb/libreboot-20241206rev8_t480_vfsp_16mb_libgfxinit_corebootfb_usqwerty.rom
```

Congratulations you did it! It was actually quite an exiting project. And I
actually did it in less than a day. However dealing with the bootorder is what caused
some extra research time and tinkering. But if you don't use Fedora Silverblue
you are pretty much good to go at this point.

## Change Bootorder

So no matter which rom you choose txtmode seabios seagrub corebootfb it doesnt
really matter, they all boot seabios first and then if it's a seabios rom it
will boot from harddrives and if it can't boot into the grub payload. If it is
a seagrub rom it does boot seabios first and then the bootorder is set to boot
into the grub payload. Txtmode roms are just without images but do the same
thing.

### Method Grub

Now if you want to use most distros you can probably get away with a regular
grub menu entry in the grub payload. I don't know where within the .rom file
the grub.cfg is. Because I couldn't use this method. For Fedora Silverblue the
Grub entries change as the UUID gets updated after each new software install be
it kernel or just a simple package. And because the grub entries not being
hardcodable would mean to reflash the rom with the new UUID each time you
install software. I'm too lazy to do that and also why.

If it was any other OS I found that the grub payload actually does a good job
in selecting if hdd encrypted OSs and boot them. In the event you still have to
create a grub entry it only requires something like this. (this is what I used
for manually booting fedora silverblue from grub, there were a bunch of other
options.

Get into the grub cmd prompt and enter those cmds (in a similar fashion with
the same cmds mas o meno you would create the grub menu entry) and then reflash
the new grub.cfg onto the bios chip with `flashprog`.

If I was lazy and left out any of the options it wouldn't boot fedora
silverblue or it would kinda boot but fail. So I took a screenshot from the
grub within the fedora silverblue when you move the arrow up/down key in the
grub menu and press 'e' for edit you get all the options that are needed.

```
set root=(nvme0n1,gpt2)
linux ($root)/ostree/fedora-UUID/vmlinuz-version.x84_64 ostree=/ostree/boot.0/fedora/UUID/0 rd.luks.uuid=luks-UUID rhgb quiet root=UUID=...actual UUID vconsole.keymap-us rootflags=subvol=root rw
initrd ($root)/ostree/fedora-UUID/initramfs-version.img
boot
```

### Method Seabios

So I read you can also change the bootorder with the `nvramtool` but not
without it also needed to be reflashed. This is a libreboot feature and could
be done otherwise.

Well the other method is to read the romfile structure, delete the bootorder
file within it and add a new bootorder file with the preferred drives using
`cbfstool` and `cbmem`. Then do an internal flash with `flashprog ... internal:
.. -w foo.rom`

As to why do all this hazzle? Well Seabios selects the first drive in the
bootorder if it fails it fails (I read somewhere that this is a BIOS limitations).
So it won't select the second entry in the bootorder and you need to restart
the machine. Now from some testing I figured out the behavior of seabios. Now
this only applies if you actually flash a seabios rom not a seagrub rom.
Seagrub always boots into grub as it is the first bootorder entry. Seabios on
the other hand selects the biggest drive it can find on the machine, it doesn't
WWAN or NVME slot. The storage size is what matters.

Now enough of the background talk let's fix this problem.


1. Boot up your newly librebooted machine in an OS that makes your life easy: I
   chose linux mint because debian basesd OS comes with the 'coreboot-utils'
   package which includes most of the commands we need.
1. Install pkg: `sudo apt install coreboot-utils`
1. Download and extract cbmem tool: https://source.puri.sm/firmware/releases/-/tree/master/tools
1. We need to get a readout of all possible pci slots written in the cryptic openfirmware standard which the bootorder needs.
  1. List all possible entries: `./cbmem -c | grep "Searching bootorder"`
  1. Now the guy who posted this fix/method on github had more luck as his full
     readout mentioned the drive names so he could identify his drive of
     choice.
  1. My output looked like this:

```
Searching bootorder for: HALT
Searching bootorder for: /rom@img/grub2
Searching bootorder for: /rom@img/memtest
Searching bootorder for: /pci@i0cf8/pci-bridge@1c/*@0
Searching bootorder for: /pci@i0cf8/pci-bridge@1d/*@0
Searching bootorder for: /pci@i0cf8/usb@14/storage@3/*@0/*@0,0
Searching bootorder for: /pci@i0cf8/usb@14/usb-*@3
Searching bootorder for: HALT
```

Now of course from all the entries the only ones that matter are the pci-bridge
entries the rest is usb slots or the grub payload or a memory test that
libreboot ships with. So because I didn't get any further information and also
`lspci | grep -i nvme` or any command that came to my mind wouldn't help to
decipher which is which. I was forced to just try and error and with a 50/50
chance of course I picked the wrong one.

However the good news is that all we have to do now is on whatever computer you
used to to the hardware flash with the raspberry pi pico you already have the
.roms with the vendor files injected. And more importantly all the tools we had
to compile `flashprog` which we will need at the end. 

Prepare:

1. Copy the '\$HOME/T480S' folder from the jumpboot/hardware flash laptop onto
   a usb stick and onto your libreboot machine (another reason I chose the same
   OS/linux mint to make sure it would work.)
1. We don't really need the original T480S/lbmk/bin/t480s_vsfp_16mb/...\*.rom
   files as you can also use `flashprog` to get a read of the bioschip and edit
   that rom and do an internal write on that. read cmd: `sudo ./flashprog -p
   internal:laptop=force_I_want_a_brick,boardmismatch=force -r mybios.bin` but
   not having to recompile flashprog is a W.

Time Safer:
1. Actually I was lazy and knew dealing with fedora silverblue to do most of
   the work required would be extra time consuming. So I just switched the nvme
   drive with linux mint from the other laptop (hardware flash laptop) into the
   librebooted laptop and it booted up with all the programs I needed.


1. In order not to mess with the original rom provided by libreboot I made a copy of the rom first:
`cp ~/T480S/lbmk/bin/t480s_vsfp_16mb/seagrub_t480s_vfsp_16mb_libgfxinit_corebootfb_usqwerty.rom ~`
1. To print the whole filestructure of the rom: `cbfstool seagrub_t480s_vfsp_16mb_libgfxinit_corebootfb_usqwerty.rom print`
1. Out of curiosity I wanted to see the original bootorder content. But you cannot simply cat the file you need to extract it first.
`cbfstool seagrub_t480s_vfsp_16mb_libgfxinit_corebootfb_usqwerty.rom extract -n bootorder -f foo; cat foo`
1. You also cannot directly edit the file within the .rom you need to delete it and then readd the file.
1. Delete the original bootloader file: `cbfstool seagrub_t480s_vfsp_16mb_libgfxinit_corebootfb_usqwerty.rom remove -n bootorder`

1. Create a new bootorder file: `vi bootorder` and add:

```
/pci@i0cf8/pci-bridge@1c/*@0
/rom@img/grub2
```

The grub2 entry was in the original bootloader file so I just left it as a
second entry but it doesn't actually do anything if the drive cant boot seabios
is stuck and so you might as well delete it. The only way to get into the grub
payload is via the `Esc` btn to get the boot options and manually select grub.

1. Add the new bootorder file back into the rom: `cbfstool
   seagrub_t480s_vfsp_16mb_libgfxinit_corebootfb_usqwerty.rom add -f bootorder
   -n bootorder -t raw`
1. I didn't have to truncate the rom to make it 16MiB as my romsize didn't
   change during this process so I skipped the `dd` cmd.
1. cd into the flashprog dir 'T480S/lbmk/elf/flashprog/' and flash the bios
   chip with the edited rom: `sudo ./flashprog -p
   internal:laptop=force_I_want_a_brick,boardmismatch=force -w
   $HOME/seagrub_t480s_vfsp_16mb_libgfxinit_corebootfb_usqwerty.rom`
1. Reboot machine
1. Now it picks the WWAN Nvme drive which is what I wanted as my fedora
   silverblue is on that one and 2280 nvme drives can be way bigger (my 2ndHDD is
   4TB) for data.

[internally]:<https://libreboot.org/docs/install/#install-via-host-cpu-internal-flashing>
[pico]:<https://libreboot.org/docs/install/spi.html>

Related:

* <https://youtu.be/iGKhsjvlSBQ>
* <https://github.com/merge/skulls/issues/37>
* <https://libreboot.org/docs/install/#install-via-host-cpu-internal-flashing>
* <https://libreboot.org/docs/linux/grub_cbfs.html>
* <https://source.puri.sm/firmware/releases/-/tree/master/tools>
* <https://mirrors.mit.edu/libreboot/stable/>
* <https://libreboot.org/download.html>
