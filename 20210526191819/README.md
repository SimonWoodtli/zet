# bboost20: Day10

Linux Beginner Boost - Day 10 (May 15, 2020)

##  Wednesday, May 15, 2020, 01:24:23PM

### Software Package Manager

***HINTS***
* enable ssh on your desktop/laptops if they crash you can keep them running an ssh into it an may be able to fix it.
* 3 ways to take snapshots and save current work on linux machines. 1.digital ocean snapshot (or smthign like that) 2.VM can do that if you put the system to sleep 3. `docker commit` creates snapshots too and are probably the easiest way of all.
* progressive web apps will destroy all those annoying app stores
* apps from OSX get mounted when you install them and drag them in to the App dir. But they just look like a disk and are mounted like a disk partition but they point at the file itself. Snaps on Linux work the same way.
apps no matter from which OS all have a common ground: they are a bunch of files(assets) with some sort of an installer configuration that have been bundled together in some way. It can be a plain directory or a compressed directory (e.g. jar in JAVA), or even OSX apps if you rightclick them you get into the dir.
* web apps are falling kind of in this category but are not packages. PWA is a webapp you can download and it will put it on you computer. It will be the nr.1 way to get apps in the future (uses your webbrowser to download) on phones.
* PWA (progressive web apps) are controlled by whoever makes them. complaint: people can be trolled because they wont know which is the real app and not the fake malware. (same argument comes from repairing iphones apple says: we can't trust some random people to fix your phones we can't trust them we don't know there skills. We need somebody who proofs to everyone else that they are trustworthy.)
* Arch AUR is amazing a network of places to download software, it boils down to having a git repo. danger: it is not vetted, just by the people using it. it's the opposite of apples appstore approach. you are responsible for installing your own software and anyone can publish anything, we just make it easy to find it.
* pacman repos are vetted by arch devs. AUR is not vetted at all, but if it works well enough it will be considered to become part of the pacman repo.
* questions: what is better let the user decide which software is good or the big companies owning their stores and making their policies to their like.
* debian you can use PPA if you want something outside the apt repo. this process is called launchpad and canonical came up with it. launchpad is github/lab for ppa

Software Package Managers: (updated will be on rwx.gg)
1. RedHat Package Manager
* yum
1. Debian Package Manager
* apt
* dpkg
1. Arch User
1. Suse
* yast
1. GUIX
1. Brew
1. Chocolatey
Language Packages
* cabal

### Understand Professional Linux Occupations

* Research Software Engineer
* Research DevOps Engineer
* Research Site/Service Reliability Engineer
* Prove Your Skills

***HINTS***
* All employment is based on trust. You should think about job opportunities and learning based on that. No one is going to hire you unless they trust that you can do what they need. And to get the trust you need to proof that you can be trusted. That means you have to understand what employer values what so you can work towards getting what they want. The government has different things they value then other employers. Everybody values things differently they trust in different things.
* Identify 2 or 3 companies you would like to work for. So think about what you want to do every day and work towards this companies requirements. Rather then go and study node because if you know node it will get you a job anywhere. Rob thinks we should flip this whole thing on its head. If you figure out what you want to do and find 2 or 3 top tier companies that do that work towards getting their trust.
* no. 1 company to work for (robs opinion): gitlab: they do great stuff and are 100% remote 2. inflexity b, they do lots of go programming based on rust. and need sysadmins
* check out mastermind's twitch stream
* most of the work as a programmer is not problem solving. but to fix problems that someone else has left you (if you work for a company). Especially if you are a beginner you are going to do testing and some task you may not like. Unless you get really good at it and you can make your own App with your own company, for a product no one else has thought of. (great way to start)
* 90% of work in the programming industry is not greenfield programming (you get to pick tech involved and make your own decisions). Most programming jobs nowadays are maintaining JAVA or Perl or C or Python. So you maintain code that already exists.
* another thing that is tough about programming: if you work on a bigger project. They are more interested how fast you can program rather then how good the program is. (Perl has a bit of a bad reputation because there is a lot of bad code out)
* you have to learn more than one coding language
* you need: gitlab, linkedin, website, thought knowledge contained in your blog (for academics) with research experiments, you are good to gig (they can trust you can do the job). If you drop those activities to update those things while you are employed. Your next job interview might not go so well. you should keep you skills sharp on the things you want to do, you need to take control and take the time to learn those things.
* machine learning is 100% college: if you want to do this it requires a huge amount of knowledge (mathskills) to do this. data science and stuff
* cybersecurity involves a bread of knowledge in addition to how computers work (for the good jobs). If you just wanna do basic bug bounty hunting, you don't even need to know how to code.
* googles SRE is a fancy word for good system admins, who automate, script and provide monitoring.
* if you look for a system admins linux. often you look for jobs with devops or SRE in the title.
* SRE are engineers that are focused on reliability and services. So google hires software engineers for linux sysadmins. Network operation center (NOC) and software operation center (soc) mixed together is called site reliability engineering. SRE= sysadmin who code
* how to keep your job or get one: 1. coding skills 2. operational skills, linux skills (code more than 1 language) for SRE
* if economy of tech company is good: money goes to app development and marketing (beat the competition), R&D etc. when the tech economy bursts the money goes: to maintain what you have, they fire software developers and kill all projects. And dump this money in maintaining what they have. And that is operations (linux sys admin stuff).
* minimum requirements for coding skills SRE: bash, python, perl, c++ or c.

who is Margaret Hamiltion: invented the term software engineering, worked at NASA, programmer

----

### General Notes

* people defaulting to docker without asking if it is the right tool for them. that happens to many technologies who get devops hype. like wordpress, why would anyone create a website dependent on a database? just to have static content? just stupid. because everybody thought databases are cool for your website. Consider always all the options!
* perl is much more effective then python on a single line command, python is bad at that
* python is good for automation
* C and go are good because they translate into software development positions. as well as software engineering positions.
* GO was specifically written for SRE-applications. It was written to do what Python, Java and C++ do in this area.
* do you need python for a full stack webdev? Rob thinks no, it will certainly help you but is not mandatory. For webdev after this course you would need to learn PHP next, because you will maintain someone elses PHP code.
* PHP was never intended to be a programming language. It was intended to be a template language that grew out of control with no management or guidance.
* first thing to learn after boost as webdev: learn a framework. Robs favourite is "few" if you wanna be taken seriously you need "react" and "few" and on top a PHP framework.

#### Common Practice

1. bb

#### Tips

1.sign up for launchpad and publish a package there (good to put on your CV)
1. https://www.browsersync.io/ lets you mess with your css code in real time on phone, tablet and have do the coding on computer while you can see changes directly! https://www.instagram.com/p/BiyDf2gAbZH/
