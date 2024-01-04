# Server Service: Add nginx

> 🤔 Consider using docker to deploy nginx

## Requirements

### BROWSER

1. Login to your favourite VPS provider
1. Login to your favourite domain name provider
1. spin up VPS
1. connect VPS IP with domainname

TODO: add link to zet here on how to do this in detail

### General Server Setup

Follow instructions on: TODO add serverinit zet here

## Install and Setup nginx

* install: `apt install nginx`
* check service: `systemctl status nginx`

### Configure Firewall

> ⚠️ Don't use `ufw` in case you are also running docker apps on your server.

* enable port 80: `ufw allow 80/tcp`

### Configure nginx

1. create config: `touch /etc/nginx/sites-available/simonwoodtli.com`

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

> 🧐 For convenience you probably want to setup a cronjob to copy your website onto the server or a github workflow.

## Next Steps

1. Setup https: add zet here
1. Secure nginx: add zet here

Related:

* <https://nginx.org/en/docs/beginners_guide.html>
* TODO add serverinit, config, ssh-hardening, and firewall, grok nginx, docker vs nginx

Tags:

      #linux #server #webserver #nginx #sysadmin
