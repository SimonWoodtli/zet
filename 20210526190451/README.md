# bboost20: Day 8

Linux Beginner Boost - Day 8 (May 13, 2020)

##  Wednesday, May 13, 2020, 01:22:25PM

### Virtualisation

***HINTS***
* Virtualbox is free, VMware costs 300$ and is less buggy
* Containerisation is not virtualisation, but it uses virtualisation
* Do you loose GPU capabilities while running virtualisation in this VM?: in 2013 with VMware and passthrough you had no issues. Because it was directly transmitted, no glitch whatsoever.
* If you do virtualisation you have to give this system at least 1 core from your CPU.
* with VM and the rising number of CPU-cores it becomes possible to run many instances and you can simulate an entire network on your own computer without touching any hardware to do so.
* Virtualbox is not so good when it comes to detecting peripherals like mouse and stuff.
* virtual machines are like “houses” and containers are like “apartments in an apartment building”. - Mike Coleman



### Container

***HINTS***
* containers do not provide storage \# not permanent
* you can say containers are apps and VM's are machines
* dockers are the fundamental core of Devops movement and Kubernetes
* you have a container with all it's complicated components, database and what not App with JSON API and graphQ and you hand this over to the "fence" to operations and they run it for you. But they run it with very specific constrains so it does not go out of control. In fact: They can take that container and make multiple versions of this container. Now you have even redundancies so the App can fall back or that another verion can run at the same time to help with the load. The mascot is a whale because it is designed like containers on a ship and a dock (that is where the idea was coming from). They want to take all the complexity of an App and put it on a ship and let it run. \# it is more than that.
* e.g. you can put and `ls` command on a docker container.
* the target for container is microservices (split up everything your organization into tiny services and web them together, they speak JSON)
* biggest letdown: containers don't provide state. They don't remember anything when they end because its an App. Unless an App has written something out to a file or communicate over the internet. It does not matter what it was running because it won't remember. no internal storage management within a container. you have to connect it to a volume (disk) to do that.
* containers are bad for databases
* with containers you can specify how much resources you are going to give it as oppose to vm's where the whole machine gets resources and the vm's OS decides how to use those as well as the software who virtualises this process. So you get a lot of control.
* you can write your own containers.
* in order to see all the dockers running on your system you do a docker PS \# this will show you the running processes. Another correlation a running container is a docker process, so to speak. And a running application on your system e.g. shell script is also a process.
* Kubernetes is a layer on top of all of this with several managers for all of this containers. Think of throwing all computers together and they get kind of clustered but not in a oldschool sense, they are all together. They have really high speed networking between them and then you give a container to Kubernetes and say: "hey I want this to run, I need it to run this much" and Kubernetes organizes where to put this container across all of those systems. So it is making sure those resources are best used by managing this process efficiently \# it is not quite a supercomputer because technically it is not sharing storage across them, usually people have another computer that is a storage array
* In a sense Kubernetes is also like an Operatingsystem as it also tells stuff where to go and manages resources to apps and what not. a strech: Kubernetes is the Operating system of containers. And Containers are running applications and processes. Kubernetes is the OS for that whole thing.
* so think of docker images like packages instead of filesystem images. you can publish them and reuse them easily

#### Setup Docker

***HINTS***
* anything you do with docker is not going to be saved.
* you can snapshot a filesystem but not the running process
* don't add people to your docker group you don't trust it's like giving them root access
* if you do ssh you need to use option: -p 22:22 (risky exposes your system)
* `adduser` and `useradd` are two different commands
* try all this editors nvi (vi), EMacs etc. in their original state
* `docker commit` saves the filesystem, it saves a new image layer (like snapshot)
* repl.it is built on containers

**Docker is awesome: because you can setup a test situation to learn a specific skill. Like learning how to setup an oppenssh. Or learning how to add a user and give it root access. Whatever you wanna learn docker is perfect for those mini tasks to learn without fear of messing your own system up.**

1. signup
1. `sudo apt install docker.io` then do `sudo docker run hello-world`
1. for raspian: https://download.docker.com/linux/debian/dists/buster/pool/stable/armhf/ download latest containerd.io, ce and ce-cli and `sudo dpkg -i filename` them. (you need to manually update them, if they get outdated!)\
1. `sudo docker run hello-world` and get positive feedback all is well
1. `sudo usermod -aG docker $USER` \# adds you to the docker group
1. now you don't need `sudo` anylonger as you have been added to the group
1. but you need to logout and login again: you can do this from the terminal without really doing it \# because that's required after adding a group
```
sudo su -
adduser xnasero \# you need 2 users to do that
sudo vi /etc/groups \# add @sudo sero, xnasero needs comma to seperate them
exit
```
`sudo su - xnasero` and then `sudo su - sero` to log back
1. `groups` should list "docker" now
1. `docker ps -a` to see a list of running dockers
1. remove the running hello-world docker: `docker rm "CONTAINER-ID"` \# docker kill or docker stop does not work
1. `docker inspect <container-id>`
1. `docker run -it -rm ubuntu` \# rm removes docker on exiting
1. `docker system prune` stops all running containers or docker container prune
1. `docker run -it -d --name sshpractice -v /home/sero:/home/sero ubuntu bash` \# -v is to mount a volume in this case your own home (be careful with this option)
1. `docker exec -it sshpractice bash`
1. now we are logged in: but there is no program installed by default so first: `apt update`
1. `useradd sero`
1. `apt install nvi` (experience how a system without vim feels like only vi 100% like original)
1. `vi /etc/passwd` add /bin/bash to the last line to the user
1. `sudo su - sero` and you should have all custom dircolor and stuff
1. `docker inspect sshpractice` in this file you can see the IP of your docker ubuntu
1. `ping this IP` shows its there `nmap this ip` shows it hast no ports (so there is no ssh on this machine)
1. `docker commit <container-id> mynewimagename` `docker commit sshpractice sshpractice1` to save the changes you made (snapshot) it stays as long as you keep it running and don't stop it.
1. exit and see `docker images` should have your saved container

----

1. `docker run -it -d --name mylinux ubuntu bash` (Full ubuntu instance with a bash shell) \# -it stands for interactive that means you can type in the app. -d keeps it running even after you exit the container, you can also reattach to it while it runs. "--name mylinux" is a chosen name by rob to refer to this container later, if you don't plan on reconnecting to it you won't need it. and ubuntu is the image and bash is the program to run (it's default for the ubuntu image).
1. Then after you disconnect it you can use `docker exec -it mylinux bash` to reconnect.
1. now you can practice adding stuff to it: e.g. openssh-server
```
apt update
apt install openssh-server
service sshd start
```
1. or maybe adding a user
```
sudo adduser $USER
sudo usermod -aG sudo $USER
```
1. Then setting up keys so you can treat it like a remote cloud server. But first you’ll need vim installed.
```
su - <you>
sudo apt install vim
mkdir ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
vi ~/.ssh/authorized_keys
```
1. You can paste your public key from another pane into the ~/.ssh/authorized_keys file. Then from another pane on your host system attempt so ssh into it after you look up the IP with the docker inspect command from the host.
```sh
docker inspect mylinux
ssh <you>@<localip>
```
1. When you are done playing around with the container, make sure when you are done to stop and optionally rm it.
```
docker ps
docker stop mylinux
docker rm mylinux
```

### Virtual Machine

***HINTS***
* you can change the settings on your Network card in VMBox from NAT (default) to Bridged Adapter or many other.
* NAT: LAN inside LAN | bridge: new pc on your LAN
* NAT saves on address allocation (because your Router does not give it an IP) which can be good since you only have 255 devices per router in a home network


NAT (Network Address Translation): 2 basic ways to think of this: imagine your computer is your homenetwork and al of your VM's are computers on your homenetwork. A computer inside a computer if you will. So nothing outside your Computer can see those VM's by default. That is basically NAT. Similar to the normal LAN where your Router protects all its Computers from the Internet does the Host computer (with NAT) make sure your VM's stay hidden from the outside.

Bridged Adapter: imagine your computer rather then making this virtual computer network like in NAT, bridged says no to this and wants this VM to treat this new created computer just like a regular computer next to the one you have (in the same LAN). It's a peer computer on the same home network.
Why does this matter? Let's say you run a minecraft server. A minecraft server with NAT to test out how it is to run your own minecraftserver. Or you create a system so you can try to hack it. For both cases you would need to set them up as bridged otherwise other computers in your Network can't see them. Because the Host Computer would say no you can't talk to that.

----

### General Notes

* snap is a mount, it's a mounted volume
* 8 cores or more and 32GB Ram are minimum for coder \# unless you only do webdev
* PS in linux runs out the running processes

#### Common Practice

1. bb

#### Tips

1. bb
