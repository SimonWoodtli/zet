# Server Service: Add nginx

> 🤔 Consider using docker to deploy nginx

## Requirements

### BROWSER

1. Login to your favourite VPS provider
1. Login to your favourite domain name provider
1. Spin up VPS
1. Connect VPS IP with domainname

TODO: add link to zet here on how to do this in detail

### General Server Setup

Follow instructions on: [20240104161046](/20240104161046/) Fresh Server: First Steps

## Install and Setup nginx

* Install: `apt install nginx`
* Check service: `systemctl status nginx`

### Configure Firewall

> ⚠️ Don't use `ufw` in case you are also running docker apps on your server.

* Enable port 80: `ufw allow 80/tcp`

### Configure nginx

1. Create config: `touch /etc/nginx/sites-available/simonwoodtli.com`

```
server {
   listen 80;
   listen [::]:80;

   root /var/www/simonwoodtli.com;
   index index.html;

   server_name simonwoodtli.com;

   location / {
       try_files $uri $uri/ =404;
   }
}
```

2. Enable site: `symlink -s /etc/nginx/sites-available/simonwoodtli.com /etc/nginx/sites-enabled/simonwoodtli.com`
2. Copy your site to server: `scp -r webdev/simonwoodtli.com/public webserver:/var/www/simonwoodtli.com`
2. Check if all good: `nginx -t`
2. Restart service: `systemctl restart nginx`

> 🧐 For convenience you probably want to setup a cronjob to copy your website
> onto the server or a github workflow.

## Next Steps

1. Setup https: [20240104162118](/20240104162118/) Add https cert for nginx
1. Secure nginx: add zet here

Related:

* <https://nginx.org/en/docs/beginners_guide.html>
* [20240104194645](/20240104194645/) Grok nginx
* [20240104161534](/20240104161534/) How to run nginx? Host vs Container
* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104134254](/20240104134254/) Server Security and Setup: Make it cosy
* [20240104124550](/20240104124550/) Server Security: Additional SSH Hardening
* [20240104130222](/20240104130222/) Server Security: Config ufw Firewall

Tags:

      #linux #server #webserver #nginx #sysadmin
