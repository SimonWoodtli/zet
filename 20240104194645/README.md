# Grok nginx

> 🧐 Nginx runs 2 processes by default a master (spawns worker processes) and a
> worker process (listens to connections and serves files to clients) `ps aux |
> grep nginx`

Fun fact: 1 nginx worker can handle 768 connections at one time. So if 1000
people at the same time try to connect to your site then one worker isn't
enough. And nginx would need to spin up a second worker process.

> 🧐 In sites-available/ you put all the sites you manage (storage). Then you symlink the ones that you want to go live to sites-enabled/

## Main features and use cases

* Reverse proxy: Nginx does this by directing the client’s request to the
  appropriate back-end server.
* Load balancing: It automatically distributes your network traffic load
  without manual configuration.
* Mail proxy server: Nginx as a proxy server for mail protocols like IMAP,POP3
  and SMTP
* Content Cache: Dynamic sites that run on Nodejs or the like can still utilize
  nginx as a content cache and reverse proxy to reduce load on application
  server and use the hosts hardware resources well. In other words nginx acts
  as a cache to help store your data for future requests.
* SSL/TLS Termination and Acceleration: The process where nginx handles the
  SSL/TLS encryption and decryption, freeing up resources on your backend
  servers. This is beneficial as SSL/TLS processing is computationally
  intensive. By handling this task, nginx can better utilize your backend
  servers, allowing them to focus on delivering content
* An API gateway: This is useful for request routing, authentication, and
  exception handling.
* Password protection for websites
* A firewall for web apps
* DDOS Protection

Related:

* https://nginx.org/en/docs/beginners_guide.html
* https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/
* https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-nginx-on-ubuntu-14-04

Tags:

      #linux #server #webserver #grok
