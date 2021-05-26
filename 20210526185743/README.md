# bboost20: Day 6

Linux Beginner Boost - Day 6 (May 11, 2020)

##  Wednesday, May 11, 2020, 01:21:03PM

### Certification

***HINTS***
* don't get a ccna certificate or comta
* don't get a certificate tight to the vendor
* don't get a LPIC3 certificate by that time you have enough skills and don't need to build trust anymore so someone would hire you.
* getting LPIC2 is saying "I am a Linux professional"


You should consider getting a certificate from [lpi](https://lpi.org). These are lifetime certificates.
the first one would be Linux essentials and if you want to become a Linux system admin consider doing LPIC2.

### HISTORY

1. free BSD is another UNIX like system developed by Berkeley University. They had make it free because it was a university project.
1. Stanford developed another one called SUN (Stanford university networking)
1. UNIX was built from AT&T, they also created (wrote) the C language specifically to rewrite UNIX in C. UNIX was at the beginnig a big company thing only.

R. Stallman wanted a truly free OS. So he created the GNU project and developed tools for UNIX under a free license. He built all the tools necessary to start creating his own version of UNIX (not sure). However the tools developed were all targeting users and not the underlining system on which it run (kernel). Mainly because this was already a lot of work and he was busy with that. That's when Linus Torvalds came into play by merging these two projects they had both tools and kernel (main piece of an OS).

### COPYRIGHT

***HINTS***
* Watch out for "NDA" in a contract you don't want that. This allows company to sue you if you take their idea or do it on your own. The danger: they might show you an idea that you already had or already done something with. And now they can claim that your idea is not something that you can work on anymore.
* All the big companies want to own you. Your intellectual property your thoughts and dreams while you are there.
* you can't copyright ideas for games.

**If you look at proprietary software in any shape or form and you wanna work for a company that is producing competing products to that company. You will be "tainted" and that means you can't work anywhere anymore.** It happens to many junior devs. that they chose the wrong project or in the wrong way, or were not fully aware of the risks on working on proprietary software.
DON'T DO:non-compete clause in a contract or NDA.

Some companies claim in contract that they own every idea you have on and off the clock. (IBM in robs case). The concept of side projects or open source projects is then problematic. You can lie about doing or they will let you know that you need to get legal approval to work on a specific open source project.

### SOFTWARE LICENSES

***HINTS***
* when you pick a license you wanna pick a license that is popular with lawyers not with people
The reason you care about licenses because they directly affect your ability to produce software and be paid.
* the decision which license you pick is largely about what you fear most. if you fear no one is going to use your software you want a very permissive license. if your fear is apple is going to steal your software, or someone is going to use my software for free and I won't see a dim about it. Then you want a non-permissive open source license.
* GPL2 is popular: because yes you can use it BUT if you make any changes you have to get them back. But it does not strip you down in additional ways like GPL3.
* if your fear is: I just don't want to mess with licenses in general then you might release it to the public domain.

Robs favourite licenses:
PERMISSIVE: APACHE2 (MIT is the popular one, but it is not popular with the people who care (lawyers))
People like the short MIT license does not give grants explicitly.

licenses protect you from two things: 1. being sued (people can sue you if they use your software and it does not work). 2. protecting your rights to the software

creative commons covers written content: sound, audio, written words but not source code (same idea).

What is the difference between free software and open source? Free software is what the free software foundation counts. Which means that if anyone makes changes they have to give it back (copy left, not permissive). Open source includes the free software license but it also includes any of the permissive licenses.  Open source means permissive licenses that allow you to sell the software or as well any changes you make without giving them back.

Linux OS is released under GPL2. However all the GNU software including bash has been released under GPL3. What is difference between them? GPL2 has 1 goal: if you make any changes I want the changes, that is it. GPL3 is GPL2 plus you have to allow anybody to access and change this software on any device ever (the owner of a device must be able to upgrade or change the software on it). The problem is this has nothing to do with the software anymore it has got something to do with the device. Legally those two licenses are not the same. And because of that GPL3 is not a replacement for GPL2.

LGPL: you can use this library as long as you link to it (old idea, modern computer languages don't work like that, they are statically linked because memory is not an issue).

#### DISTROS

***HINTS***
* Redhat vs. Ubuntu vs. Suse compete in the Server sector

Quebes OS = snowden recommends
Vanilla Debian = nice but not friendly for proprietary hardware
Endeavour OS = Arch based but beginner friendly
Arch
CentOS = Redhat, for server
Fedora = Redhat (big companies use that)
parrot
Kali = Hacker OS (but real hackers make their custom Arch)
Linux Mint cinnemon = very stable, beginner friendly
Pop OS = Robs recommendation for beginners
Gentoo = you need to compile everything (no package manager)

----

### General Notes

* Android is an applications frameworker that runs on top of Linux. Hence Android=Linux
* Apple campaigns against Linux, e.g. removed bash and replaced it with zshell.
* Windows implements Ubuntu and is getting a really reliable, fast access to it.
#### Common Practice
1. bb

#### Tips

1. If you install 5 different OS to VM + 2-3 Cloudbased OS + 5 on hardware (both server&desktop) you can get familiar with different flavours. You should at least do: install 1 package and 1 daemon/service for each OS.
1. getting minimal docker and container knowledge is essential
