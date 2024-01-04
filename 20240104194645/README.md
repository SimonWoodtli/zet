# Grok nginx

> 🧐 Nginx runs 2 processes by default a master (spawns worker processes) and a
> worker process (listens to connections and serves files to clients) `ps aux |
> grep nginx`

Fun fact: 1 nginx worker can handle 768 connections at one time. So if 1000
people at the same time try to connect to your site then one worker isn't
enough. And nginx would need to spin up a second worker process.

> 🧐 In sites-available/ you put all the sites you manage (storage). Then you symlink the ones that you want to go live to sites-enabled/

TODO: research more and add some more content

Related:

* TODO add
*

Tags:

      #linux #server #webserver #grok
