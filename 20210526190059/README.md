# bboost20: Day 7

Linux Beginner Boost - Day 7 (May 12, 2020)

##  Wednesday, May 12, 2020, 01:21:53PM

### Cloud Services

***HINTS***
* Book learning: comprehensive Project learning: address boredom \# you need both
* AWS try it its free. But the big cloud services are more for businesses not for beginner coders to learn
* Rob recommends Digital Ocean and Linode for beginners
* Security is very important if you want to use Linux hosting as you are responsible for it.
* Digital Ocean is used by hackers: because it is so easy to setup servers there
* Why Cloud Hosting Provider: 1. if you wanna be secure at your LAN, Problem: if your homeserver gets compromised the whole LAN is at risk. 2. Better uptime, Computer is directly connected to backbone of Internet -> high bandwidth
* best way to learn about these services is to create an account and figure out how they work. \# watch your bill!
* scoop is like apt but for windows

1. Software as a Service: e.g. Microsoft 360 and the like
1. Platform as a Service: e.g. IOS appstore if you have app and want to release it somewhere
1. Infrastructure as a Service: e.g. linux hosting \# servers that you need to maintain and update by yourself u get barebone servers
1. Function as a Service: you make App but don't put it on Platform so it runs in a Webbrowser or somewhere else. When you need to make changes you call out to their function so they make the changes in the Backend. \# some stuff like this is free on Netlify. NEEDS API
1. Database as a Service: leader: fauna.db if you want to run your own data
1. Collocation as a Service: they provide hardware so you run your server installed at your LAN but they take care of config and all.

**make your own cloud**
Docker
Quebernetes
DevOps \# this job is like you are AWS for a company and you help them configure their own cloud
Containers

#### Linux Hosting Providers

***HINTS***
* D.O. has super easy interface on web
* Heroku is PaaS they run Apps \# great for that
* Glitch.com is also PaaS as well Netlify
* You cant install software on Netlify just run Apps or your Website \# amazon lambda functions, Passwortfrontend
* To-DO: go trough all the majors providers and install linux servers on them, you can put this on your CV. Or say in interview: I used all of them but prefer this one for that reason. Also create a little project on them not just installing the server, like the minecraft server we did.
* AWS hosting: start with EC2 later you can do S3 or R53
* Challenge: create two hosted servers and ssh from one server to the other

Meet Amazon Web Services (leader on linux hosting)
Meet Google Compute Engine
Meet Microsoft Azure
Meet Digital Ocean \#if deactivated you can turn it on later again \# rob runs DNS-Server there
RackSpace
Meet Linode
Grok Appeal of Hosting APIs

**What can you do on them?**
Host an Agar.io Clone
Host a Doom server
Host Your Own Websockets Game
Host Your Own Database with GraphQL API
Grok Why Hosting Web Site is Bad Idea

####Create a Digital Ocean Droplet

***HINTS***
* Backup option is not necessary, you can do this on your own with rsync
* VPC: You can only connect to your Server if you are in a VPN connection with it. \# it makes your server invisible to the internet.
* X Forwarding= makes it possible to run graphic applications from your remote system
* don't do stuff as root, always create a user for your servers \# dangerous -> mistakes are not forgiven
* NEVER DO THIS: on your server: `vi /etc/ssh/sshd_config or nano /etc/ssh/sshd_config` change "PasswordAuthentication no" to "yes". Rerun the service: `sudo service sshd reload` it's better to create a user instead.
* for servers rob does not recommend to get your dotfiles Repo from gitlab on there. Because this will grow and bloat the server with many things that are not neccessary
* pam is an additional security layer that is on top of the traditional security from linux/unix. \# pam.conf
* Windows uses WLS2 for server stuff? what is that?
* 3 reason to use TMUX: you get persistence for your connection, you get the window management, and you get concurrent access to the same TTY (same screen), so many people can login to this server shell at the same time

1. create droplet 1GB ubuntu 20.04 LTS \# don't do VNC or Backup
1. add your ssh pub key `cat ~/.ssh/id_ed25519.pub` to it
1. change hostname to bboost \# it shows up in your commandline prompt
1. finish setup of droplet and copy IP
1. `ping IP` to see if you can connect to it
1. `ssh root@SERVERIP` to connect `ssh -l root 134.122.11.212`

**Following commands from your local username:**
1. `scp ~/.bashrc root@134.122.11.212:` \# don't forget colomn at end or it copies it to your local ~
1. `scp ~/.vimrc root@SERVERIP:` \# don't add TMUX, we don't need this for this server use, we can run TMUX locally better idead
1. `scp ~/.dircolors root@SERVERIP:`
1. `scp ~/.bash_aliases root@SERVERIP:` \# only Rob needs to cp his .bashrc, but we need aliases because our .bashrc is default and all our settings are in aliases
1.
1. create `touch sshconfig` in your Repos/rwxuser/dotfiles
1. add: # put this in your ~/.ssh/config \# later when you created a user on the server you can add this user (xnasero) to this file instead of root

  Host bboost
     User root
     Hostname 161.35.184.255
     IdentityFile ~/.ssh/id_ed25519
1. create another softlink to ~/.ssh/config so edit your Setup `vi Setup`
1. now you can simply use `ssh bboost` to login \# following commands on server
1. `vi /etc/ssh/sshd_config` check if "PasswordAuthentication no" (never YES!)
1. `adduser xnasero`
1. this user needs now root access: `vi /etc/group` and add to the sudo line at the end xnasero
1. `su - xnasero` \# changes user
1. `mkdir .ssh` then `chmod 700 .ssh` \# only readable by you `touch .ssh/authorized_keys`
1. get your ssh key `cat ~/.ssh/id_ed25519.pub` \# local user
1. add the key to server: `vi .ssh/authorized_keys`
1. add your configurations to the new user `scp .bashrc .vimrc .bash_aliases .dircolors bboost:`

#### Setup Paper Minecraft Server on your Droplet

***HINTS***
* TMUX needs to be installed on some systems `screen` is on everything.
* TMUX default is CTRL-B rob configured it so it uses CTRL-A, so if you accidentally run two TMUX sessions locally and on the server. but it is not configured on the server you can still use CTRL-B to access it properly
1. login: `ssh bboost`
1. `1curl -L -o paper.mc https://papermc.io/api/v1/paper/1.15.2/282/download` \# o option is to rename the file
1. check what file it is: `file paper.mc` (should be zip)
1. `java -jar paper.mc` instal java: `sudo apt install default-jre`
1. `java -jar paper.mc` \# not secure, just because we are beginners, you should use safety options to run a minecraft server, jar are zip files
1. `vi eula.txt` change eula=false to true
1. `mv paper.mc paper.jar` \# we named it wrong
1. `java -jar paper.jar`
1. if you exit terminal server dies!
1. to fix this we need to use `screen` or `tmux` because tmux is already installed we use that
1. `scp .tmux.conf bboost:` \# from local shell
1. `tmux` to run tmux `java -jar paper.jar` and now CTRL-A-D will close session but your minecraft server still runs

----

### General Notes

* modern webdesign: is heavy on frontend and light on backend, does not get generated on the server and backends are queried.
* Email (@) came from syntax like root@serverip \# see Create Digital Ocean Droplet

#### Common Practice

1. bb

#### Tips

1. It makes more sense to use Netlify for your website than to use IaaS and run a Linux server with your website on it. Because you cant hack the website, you need to hack Netlify. Another reason: website gets distributed on a CDN \# content delivery nodes, which means it puts it closer to people who access your site \# less lagg. To setup this on your own is like to setup Cloudflare or the like \# lot of work.
