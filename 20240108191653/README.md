# Server Service: Add yt-local with nginx proxy and pw auth

## Install yt-local

1. Install dependencies: `apt install python3-venv`
1. Create dir: `mkdir ~/Prod`
1. Get tarball: `curl -LJO https://git.sr.ht/~heckyel/yt-local/archive/v0.3.2.tar.gz`
1. Extract: `tar xvf yt-local-v0.3.2.tar.gz`
1. Rename: `mv yt-local-v0.3.2 ~/Prod/yt-local`
1. Create virt env: `cd ~/Prod/yt-local && python3 -m venv venv`
1. Source venv: `source venv/bin/activate`
1. Install pip dependencies: `pip install -r requirements.txt`
1. Check if it works: `python server.py` (Ctrl-C, if all good)
    1. If you plan to host it without running your webserver in docker you're good to go
    1. If you are running your web/proxyserver within docker you need to edit the ~/.yt-local/settings.txt file (see details later)
1. Exit virt env: `deactivate`

## Setup yt-local

### Create a systemd service to auto start the flask server:

1. Create new service file: `vi /etc/systemd/system/yt-local.service`

```
[Unit]
Description=yt-local - YouTube Frontend
Documentation=https://git.sr.ht/~heckyel/yt-local/
After=network-online.target

[Service]
User=xnasero
Group=xnasero
ExecStart=/home/xnasero/Prod/yt-local/venv/bin/python /home/xnasero/Prod/yt-local/server.py
Restart=always

[Install]
WantedBy=multi-user.target
```

2. Load newly created service: `systemctl daemon-reload`
2. Enable it: `systemctl enable yt-local.service --now`
2. Check it: `sytemctl status yt-local.service`

### If your webserver/proxy server is running inside a container

Using this app with nginx proxy manager(NPM): The Problem

The yt-local flask app starts by default on the 127.0.0.1:9010 loop back
interface. And it's meant to run on the host machine directly. However NPM runs
as a docker container. NPM cannot access an app that runs on the host if it's
on 127.0.0.1. That's because for NPM 127.0.0.1 is the loopback interface inside
docker. This is also true for any other webserver that runs inside a container.

The solution:  
Make yt-local run on 0.0.0.0 which means any machine on LAN can access it. And
NPM can also redirect it because NPM can redirect any app running in the LAN
when you point it to their local IP given by the router. So you'd just use
192.168.1.44 (or whatever your machines IP is that runs yt-local) for your
proxy host.

IMPORTANT: don't create the settings.txt file into the yt-local project! Use the ~/.yt-local/settings.txt file instead!

1. Edit the yt-local settings.txt file: `vi ~/.yt-local/settings.txt`
1. change to faulty booleans to true 'allow_foreign_addresses = True' and 'allow_foreign_post_requests = True'
1. Now if you start the server `python server.py` it should start it on 0.0.0.0:9010

### If you run nginx directly on the host: Create nginx proxy for the app

1. Create proxy site: `vi /etc/nginx/sites-available/yt`

```
server {
        listen 80 ;
        listen [::]:80 ;
        server_name yt.simonwoodtli.com ;
        root /var/www/mysite ;
        index index.html ;
        location / {
                proxy_pass http://127.0.0.1:9010 ;
        }
}
```

2. Enable proxy site: `ln -s /etc/nginx/sites-available/tv /etc/nginx/sites-enabled/`
2. Restart nginx: `systemctl restart nginx`
2. Check if it works in browser: `yt.simonwoodtli.com`
2. Add https cert: `certbot --nginx` and select tv site number

## Next Steps

* Add nginx pw auth: [20240108005229](/20240108005229/) Server Service: Add password authentication to nginx site

Related:

* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104162118](/20240104162118/) Add https cert for nginx

Tags:

      #linux #server #youtube #flask #nginx #proxy #systemd
