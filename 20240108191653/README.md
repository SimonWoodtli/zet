# Server Service: Add yt-local YT frontend

## Install yt-local

1. Install dependencies: `apt install python3-venv`
1. Create dir: `mkdir ~/Prod`
1. Clone project into Prod/: `git clone --depth 1 https://git.sr.ht/~heckyel/yt-local -C ~/Prod`
1. Create virt env: `cd ~/Prod/yt-local && python3 -m venv env`
1. Source env: `source env/bin/activate`
1. Install pip dependencies: `pip install -r requirements.txt`
1. Check if it works: `python server.py` (Ctrl-C, if all good)
1. Exit virt env: `deactivate`

## Setup yt-local

### Create a systemd service to auto start the flask server:

1. Copy any systemd service file: `cp /etc/systemd/system/smartd.service /etc/systemd/system/yt-local.service`
    1. Edit it: `vi /etc/systemd/system/yt-local.service`
1. Load newly created service: `systemctl daemon-reload`
1. Enable it: `systemctl enable yt-local.service --now`
1. Check it: `sytemctl status yt-local.service`

### Create nginx proxy for the app:

1. Create proxy site: `vi /etc/nginx/sites-available/tv`

```
server {
        listen 80 ;
        listen [::]:80 ;
        server_name tv.simonwoodtli.com ;
        root /var/www/mysite ;
        index index.html ;
        location / {
                proxy_pass http://127.0.0.1:9010 ;
        }
}
```

2. Enable proxy site: `ln -s /etc/nginx/sites-available/tv /etc/nginx/sites-enabled/`
2. Restart nginx: `systemctl restart nginx`
2. Check if it works in browser: `tv.simonwoodtli.com`
2. Add https cert: `certbot --nginx` and select tv site number

## Next Steps

* Add nginx pw auth: [20240108005229](/20240108005229/) Server Service: Add password authentication to nginx site

Related:

* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104162118](/20240104162118/) Add https cert for nginx

Tags:

      #linux #server #youtube #flask #nginx #proxy #systemd
