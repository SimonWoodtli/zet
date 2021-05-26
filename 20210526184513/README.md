# bboost20: Day 5

Linux Beginner Boost - Day 5 (May 8, 2020)

##  Wednesday, May 08, 2020, 01:20:14PM

### NETWORKING

***HINTS***
* You need to know: - how the internet works - how does your homenetwork work

#### How does the internet work?

It's like a really fast postal system. (very oversimplified) Basically all of our houses are the equivalent of computers on the internet (devices, phones etc.) can be compared to our homes. The mail that we sent (real world) is the system. You have to write the address on the package in order to send it to a house. But it doesn't go there directly it goes to a post office and then further maybe to a public postbox where you can collect it or your house. But it doesn't matter where it exactly goes (the sender does not care) as long as there is an address on it we send it. We care only about: it gets there in one piece, it doesn't get stolen, it doesn't get broken or dropped. Then the package goes to the post office. The 1. post office is like your router (most routers have firewall rules protecting you, NAT). Everybody needs a router it's connected to the internet. Most people have two pieces. You have the WIFI Router that receives the first package and that immediately hands it off to your cable modem, that's kind of like another post office in the same building. And then it's like ok you get this now, you get all the packages but I just process the packages for you now, I'm gonna give you the package. And then that gets the package.
And what it actually does it connects it to - all the mail from this post office is all destined for one central post office. And that is the same as thinking of your cable modem connecting to it's first connection, which is your internet service provider (ISP). And they have a router that receives that information as well, and so on and so on... They have a way of sorting it at that main place and then they have ways to sorting it at the next place. And then they have ways of sorting it at the next place. And every step along the way the person receiving the package looks at the address of the package and they sort it. And they say ok, this goes over here. Sometimes they will even write on it. And that is actually very similar to how packages work. Sometimes they put it in another package. You know essentially they put it in a group, like with slots to organize it all. And so all this mail travels all over the place without you really knowing how it gets there. And there is a really important comparison here. If one of the post offices gets taken out, unless it's your post office, your actual 1. post office the mail will still make it. And the reason for that is because the people that are sending your mail are gonna find another way to get where it needs to go. So that is what is happening on the internet, with all your data. Anything you do on the internet gets converted into packages and travels on the internet at lightspeed. And anytime you can see a blink on the router you can kind of thing of it as if mail is being sent.

**TAKEAWAY** you have this metaphor of the post office system and different kinds of mail. Stuff that has to have re answers to prove it made it there (TCP/IP). Postcards that you would just give out as many as possible and don't really care if we get answers (UDP). And we have ICMP which tells us: what's going on? ping, traceroute and tracepath use that.

Based on that: Let's say we're going to send an email at my domain. And you need a way to look up how to send it. So the address that you are putting on the package is rwx@robs.io (.io is called a domain name, same for websites). The domain names are an extra piece of that they are helping us be human and not just have to use numbers. Every single domain name resolves to one or more IP numbers. And the IP number is the number that translates into a bunch of 1's and 0's (bits) that the computerized post office uses to decide where your thing goes next.

e.g (older one to digs is `nslookup`) `digs robs.io` shows that you have an A-record that is a type of address in the domain name system. The domain name system is a system that lets you look up these numbers, and it happens really fast. And that tells us anytime that I want to send a package to robs.io I am actually sending it to this IP 104.198.14.52.

Lets send some packages (ping sends packages using a protocol called ICMP, which is just a protocol designed to watch and monitor and see if it works, and sometimes it's blocked.(NOT TCIP): `ping robs.io`
`ping 8.8.8.8` this is the address of a computer at google (DNS) (over understatement) that gives you back the IP number that goes with a name.
So like we did with robs.io that gives you back the number. (super fast for a computer on the internet)

`traceroute rwx.gg` (old thing) `tracepath rwx.gg` (new one): it is a tool that uses the ICMP protocol to effectively ping each thing along the way and see what comes next. and there is lots of blank holes in there that's because there are devices that are doing things that are not playing ICMP they are like no I refuse (because these tools for discovering systems and hosts are used by hackers all the time). Tracepath shows you the response time between each segment (super cool)
`pathping -n` does the same
traceroute often doesn't really know what is there.

`ip -br a` to look up your own IP (don't forget br or you get way too much info)
shows you all the connections that your computer has to the internet.

TCIP is the important protocol that controls most of your communication. There is another one called UDP which is used for gaming. The only difference between them is UDP does not care about getting an acknowledgement of having been sent. And you can compare that to mail when you have returned receipt, when you send a package and you say I want returned receipt, in other words the post office notifies you when it's got the package (it's a lot slower to do that). That is kind of the equivalent to TCP/IP. What it does it wraps up the IP package and says I want you to tell me when you got it (called ?finway? acknowledgement) and then it comes back. And that can be a problem what if someone decides to not answer you? How long do you wait? Because of that the TCP/IP protocol is not used where intense communication is required and you really don't care about the answer. Because there is going to be a new package before you would even get done asking the question. About whether there is another package (streaming is a great example). So it's like here is a ton of packages hey if you missed one just draw the next one. Any kind of realtime gaming or streaming uses UDP. It's kind of like having a different way to use the same post office. With UDP is a postcard if you don't get the postcard no one is going to die. And then all the other mail is like returned receipt. It's like I am going to send you a package and you are going to tell me you got the package or I am going to sit here and wait until you tell me.

There is this other protocol called ICMP and that is like hey I am just here checking on the system here I just who the next router is. I just wanna see if you are even out there. And those are the only transport protocols that you really need to understand.

The IP number on showmyip.com is for your building. The IP address on `ip -br a` is for your computer. The way the mail(packages) get routed is that they get sent to the router that is listening at that IP that you saw on showipcom.com and that IP gets the package and goes Ohh lets see where it goes here. And most of the time it does not go anywhere. Unless its an answer/response from something that came from inside of the network. So what NAT does is: it sends out a package from your computer at home and that goes to the router before it hits the internet and then it goes out into the internet and then some services on the internet answers back. And sends back a package. And your home router remembers what package that was and goes like you know what, that is a response to this guy. Imaging that our postal system kept track of every single package that left and knew who to send it back to without knowing anything more about it. So this allows our homenetwork to send out information to the internet and make sure it comes back to us. Without giving it any real hints.


Problem: What if someone out in the internet wants to send your specific computer package, it can't do it. why not? because the router doesn't have anything to go on. Essence of attacking a network: So your router gets lets say some random random computer out there wants to send something to you. Maybe it wants to talk to your computer specifically. Maybe it wants to SSH. But it can't do it. It says ok I want this IP address but if they get to your home router. Your home router would be like: I never seen this package before I have no idea where it goes. And other than it requesting a specific place (a door or also called port). It has no way. So it like I am sorry and is going to throw it away. Unless you tell your router hey send all this stuff that we don't know about to this particular machine, it will throw it away. And that is how it is protecting you from unwanted incoming connections (DMC) because it doesn't know how to do that. DMC is kind of a no man zone between your router and the connection to the internet, where you can also link other devices that can be more exposed to security risks. You are nice and safe in your home network because your router is protecting you. And you don't wanna mess with that.

#### Homenetwork

The metaphor for the internet sort of also relates to your homenetwork. But it's more like a Building with a Mailroom downstairs. So packages send from one device to another go to the mailroom but it never hits the internet.

Joke: There is no place like 127.0.0.1 because in the old days is a special ip number that has been given to your computer. It's called a loop back interface (lo=identifier). This connection actually only goes to your computer. You can think of your computer itself as its own network. (there lives a network inside your computer). And this is actually even more true if you have a virtual machine running. (vmnet-number= identifier)

* Interface= connection to the network
* Localhost:3000 = 127.0.0.1:3000 any communication with this ip never leaves your computer! There is no domain name for it. At one point it was called Localhome hence the joke.
* wlps= wlan interface this is a connection to your home network. It shows the IP number that your router has decided to give you. The router makes that decision with his own protocol called dynamic host control protocol (DHCP). There is another form of receiving this IP addresses and that is called static (rarely used these days). Almost always even though the number does not change that often you are using DHCP. If you wanna make a server: you need a static IP (like my pihole). If someone logs into your WIFI with a device he will get a random new number and that number will change (hence dynamic).
* On your Computer you can model thanks to its own network all kinds of things (in tiny capsulated spaces inside a virtual machine).
* The IP you get from pages like showmyip.com is called NAT (Network Addressed Translation). You can think of it as being the address to your building. Your home network all has it's own IP's start usually with 192. or 10. (internally reserved IP-ranges that are only for internal use. 10. is a much larger range therefor it's used for companies routers with thousands or millions of devices on the network. On your homenetwork it's usually 192. (by default only provides 255 devices).
* IPV6 is encoded with BinHex

#### Port

Closed thing that we can compare it to our metaphor is the door (it french port=door). So lets say you send a package somewhere or someone sends you one and it comes to your building and you have 2 or 3 or 4 doors. Maybe you have a service entrance and a special entrance for VIP and a main entrance. And the postman comes to your building and they have a package and they don't know where it goes. They don't know which door to take it to (by default they might take it to one). A port is the number above the door. And there are specific ports that have been signed in advance, so everyone agrees up on it. e.g. port 80 so anytime you visit a website you can add `:80` at the end of it and if they support non secure https.
There are two ports that are really important for the web. Port 80(HTTP) and port 443 (HTTPS, encrypted secure version) everyone should use for the most part.
This means when our package comes to the door of the building (it got there because it knows the IP) it then says ok where do I go now? It looks at the building and sees ah there is 443 over there it has guards on it (that's for secure) so it takes it to the secure and gives the security door the package.
If it's not it goes to the main HTTP door
If its 22 (SSH) it goes there. And you notice something when you do something over the wiring and you're hacking stuff like that.
Any number above 1023 is non privileged that means that any applications out there can just decide it wants to "bind" to that port. That means listen for newcoming connections on that port. Without a problem. Anything under 1024 needs root access to your system. That is why robs computer chose the random port 3000 for his locally displayable website.
`vi /etc/services` shows a list of all the ports. So if anyone wants to connect to you you can see where. You can also use :http instead of :80 (so the name works too)
IRC port: 6667
Minecraft port 25565

Malware runs on different ports too. When you see a lot of things trying to connect to a lot of different ports you will notice that something is up and you might being hacked. And you can figure out what based on what it is trying to connect to.

`ifconfig`
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.15  netmask 255.255.255.0  broadcast 192.168.0.255
        ether xx:xx:xx:xx:xx:xx  txqueuelen 1000  (Ethernet)
        RX packets 33444  bytes 32061773 (30.5 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 23857  bytes 3814394 (3.6 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
netmask: tells you have many numbers you can use.
broadcast: is if you want to send something to everybody
ether: Mac-adress or ARP-protocol


#### ARP-Protocol/Mac-address

* There is also the ARP-Protocol which is the new name for MAC-Address. Which is also sort of assigned and can be changed so in this world of VM's you can change it.
* The MAC-Address which is a machine address, had the idea to be a way to always address one specific device. (in the old days this was literally burned on to the network device on your computer), so its a hardware address.

The mac address is like the definitive way to send a package to a specific computer. And both of those addresses are on the package. You can imagine the package receiving a new stamp everytime it goes to the post office. So the first stamp it gets is: you need to go to my own post office so the address of the post office is written on that. And then that post office gets it and effectively crosses the address out and writes the address of the next place to go. And so on. Now the IP on the package does not change the destination address never changes but all the intermediate addresses that are added along the way are. That is what is going on with the MAC-address.

#### Attacks

ARP-Attack: is flooding the local Network with messages saying "yeah I am the one, I have that address", "That's my MAC-address", "That's my MAC-address" ... and the machine who actually has the MAC-Address is over there and he doesn't answer very much. And so the ARP-attack is like saying hey I got it i swear and then it will mess up your LAN.

So this is one of the way to mess up your network is so that it doesn't know what is going on. And then none of your computers know what is going on they don't know who to talk to. They don't have to talk to the router for everything. They should be able to talk to each other directly if you mess on your Local Area Network (LAN).

Package storms can really mess up your Network. By messing up your switches that's why Network engineers make so much money.

Man in the middle attack is usually started by flooding the Network with those messages so the computers who are on the network think you are the router. And all of a sudden they send their packages to you and once they catch that they are fine to do it as long as they get responses. And then you forward the traffic to the router and you look at all the traffic and do whatever you want with it. And you forward the responses from the router back to the computers. Basically your computer/hacker device becomes a man in the middle router and you can anything. To avoid seeing too much you should always use HTTPS because these are still encrypted and cannot be seen even if there is such an Attack.

This is also the reason when you set up an ssh-key it asks you: are you sure you trust this IP-address? Because if there is any chance at all of a man in the middle attack you will give this information to them instead of the service that you want to use. That is one of the weaknesses of initial ssh. which you can get around with through certificate signing (complicated).

#### SSH (secure shell)

 ssh (SSH client) is a program for logging into a remote machine and for
     executing commands on a remote machine.  It is intended to provide secure
     encrypted communications between two untrusted hosts over an insecure
     network.  X11 connections, arbitrary TCP ports and UNIX-domain sockets
     can also be forwarded over the secure channel.

**Do the first 5 lvl of overthewire.org/wargames/bandit**
It is basically a Terminal on a remote system. You have a Terminal and you ssh into another remote terminal to get a connection to that system.

SCP is a way of transfering files that uses SSH. `scp programname sshname:`

#### VPN

Don't tell anyone which VPN-service you use. If you recommend it to someone tell something you don't use. Basic privacy keeps you from being attacked and then there is secretcy. There is a difference between them. Privacy keeps freedom alive, keeps journalist alive it's a good thing. Secrecy can be good or bad it is used so people can hide crimes and bad stuff. Don't mix those two up.
People go like: Oh well you got anything to hide? If not what's the problem? But neither do most journalist in the world. It's not just about you it's everyone needs it. Problem today: people who go with privacy are automatically suspected of hiding things (secrecy) and that makes them suspects.

How the government gets hackers: Lets track all the VPNS within that radius for a 100miles. From every coffee shop within the last year. And then train you like that and centralize their search and they catch people.

If you think you can be anonymous you should stop right now! You can do the best to protect your privacy but somebody who wants to find you will find you.


### Learning to Learn

SUNDAY 11-12AM Coffetalk on Discord you can join and talk with the people there. (they are also on youtube)

If you have knowledge about something a good way to show/proof it is by doing a project. How can you convince yourself that you truly understood something you have been working on? Writing about it is the answer. You can also show people your notebook about this topic. Take what is in your notebook, take some of it out of there and make it into something that is public. Keep a repository of your examples. Look at davinci his notebooks were filled with wild idead. Did he do that for someone else? No...

Part of being autodidact: is writing and drawing so that you can recall it and maybe others too.

\#autodidact on twitter check out robs twitter
dogma= when you are so set in your ways to refuse to listen to anyone elses opinion or have your opinions challenged. You get defensive and angry about it. This is normal to some degree as we defend our worldview if we see it attacked (backfire effect). But what is really dangerous is won't listen to anyone under any circumstances. That is death - no progress can happen. Not only for you but also for others.

----

### General Notes

* the twisting of the pairs in a cat5 cable cancels the EMI (electromagnetic interference) and was a major breaktrough in wiring and making the internet a thing. Because the impedence (degrading of the signal over lenght) was much less than before. Also The twisting of the 4 pairs is so that they don't interfere with each other.
* The internet is a network of networks
* if a local network does not allow port 22 for ssh. You can't access servers on that network however you can run an ssh server within this network on a normally web port like 443. And then you ssh trough that. And this network will think ah that is just more web traffic and allows it.
* People feel attacked if you attack their ideas.
* typescript= part of javascript (most of netflix runs on typescript)
* netstat is deprecated, ss is its successor

#### Common Practice

1. Use `ping website` to check whether your internet connection is working (most simple way). It's also telling you how fast it is.
1. Port 21 is Telnet (insecure, dont use)
1. Call it MAC-adress as it is a custom to do so. Or Mac/Arp to be more precise.
#### Tips
1. buy alpha dongle: allows you to understand wifi better and get more information out of your own wifi and others.
1. Why should you use a content delivery network (rwx.gg uses it): it puts the content closer to you on one of the 3, 4, 800 servers. (makes it quick and responsive, what netlify does). Why you should be using netlify. Also that is why your IP for netlify is going to be different than someone elses. It allows for the webcontent to be closer to you. As opposed to putting your content at a homeserver, you would have to connect to that one, and only that one. And that is the advantage of sort of a jamstacking approach.
1. Learn BinHex: counts from 0-9 and then a-e to handle numbers 11,12,13,14,15 and 16. You can also learn Octal
1. Always change the routers login: username or at least password. Anyone attacking you can attack your router and that is the most that they can attack unless you allow them to attack more explicitly. Unless you go in and say I want all the packages for secure shell that go the server. I want to allow minecraft to come through. Anyone that tries to connect to an incoming minecraft connection - I want it to go to that computer that is running my minecraft server.
1. robs `myip` script is a nice safe way to look up your IP that goes to the internet.
1. Use a VM or go to your local community college to learn about Network and experiment with it. They can set up scenarios where you can break the network by flooding it with packets.
1. SCP is a most have in your tool set. Because you can quickly copy files to a remote system via SSH and vis versa. rsync on the other hand keeps entire dir in synch or if you want permissions to match (use rsynch to set up a back up)
