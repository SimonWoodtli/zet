# Server Service: Add Simple Login


## Prerequisites

* Create a domain name:
    * IMPORTANT: This domain will be the main domain name for every user created on this SL instance
    * Use a paid domain as they always allow to set any records.
    * For a more anonymous setup use a free DNS: desec.io, ydns.io
    * desec.io currently doesn't allow new acc creation (maybe that will change in the future). You can only use their DNS redirection.
* Have an ubuntu LTS VPS or debian 12 server up and hardened: min. 1GB ram
    * While creating your VPS in the browser, use hostname: 'sl.yourCustomDomain.com' or 'sl.foo.dedyn.io'
    * Open ticket to get VPS provider to open port 25
    * Create user 'sl' and add it to sudo group
    * [20240104161046](/20240104161046/) Fresh Server: First Steps
    * [20240104124550](/20240104124550/) Server Security: Additional SSH Hardening
    * [20240104010508](/20240104010508/) Server Security: Add fail2ban
    * [20240104130222](/20240104130222/) Server Security: Config ufw Firewall

## Install Simple Login

1. login: `ssh sl@VPS_IP`
1. check domainname: `dnsdomainname` and fully qualified domain name are set correct: `dnsdomainname -f`
    1. domainname: 'foo.dedyn.io' and fully qualified: 'sl.foo.dedyn.io'
    1. 'yourCustomDomain.com' and 'sl.yourCustomDomain.com'
    1. If not correct: Change hostname `hostnamectl set-hostname sl.foo.dedyn.io`. Add new line: 'VPS_PUBLIC_IP4 sl.foo.dedyn.io' to /etc/hosts (maybe also 127.0.1.1 needs to be changed)
1. Open ports:

```
sudo ufw allow 25
sudo ufw allow http
sudo ufw allow https
```

4. Install pkg: `apt install dnsutils`
4. Create dirs: `mkdir -p ~/sl/{pgp,db,upload}`
4. Generate domain keys: `openssl genrsa -out dkim.key 1024` and `openssl rsa -in dkim.key -pubout -out dkim.pub.key`
    * IMPORTANT: If `openssl version` is 3 use: `openssl genrsa -traditional -out dkim.key 1024`
4. Restart Server: `sudo reboot`
4. Create 7 DNS records on your either free or paid DNS provider:

```
A-Record:
  name/host: foo.dedyn.io
  answer/point to: VPS_IP4

AAA-Record:
  name/host: foo.dedyn.io
  answer/point to: VPS_IP6

CNAME-RECORD:
  name/host: *.foo.dedyn.io
  answer/point to: foo.dedyn.io

MX-record:
  name/host: foo.dedyn.io (or leave blank same effect)
  answer/point to: sl.foo.dedyn.io
  priority: 10

TXT-record:
  subname: dkim._domainkey or if host: dkim._domainkey.foo.dedyn.io
  value: output of sed ("" quote the value)
  sed "s/-----BEGIN PUBLIC KEY-----/v=DKIM1; k=rsa; p=/g" $(pwd)/dkim.pub.key | sed 's/-----END PUBLIC KEY-----//g' |tr -d '\n' | awk 1

TXT-record:
  name/host: foo.dedyn.io (or leave blank same effect)
  value: v=spf1 mx ~all

TXT-record:
  subname: _dmarc or if name/host: _dmarc.foo.dedyn.io
  value: v=DMARC1; p=quarantine; adkim=r; aspf=r
```

9. Verify Domain Response: (might take a while for DNS to update new records). Use either `dig` or `nslookup`

```
dig @1.1.1.1 sl.foo.dedyin.io a
dig @1.1.1.1 foo.dedyin.io mx
dig @1.1.1.1 foo.dedyin.io mx
nslookup -type=TXT dkim._domainkey.foo.dedyn.io
nslookup -type=TXT foo.dedyn.io
nslookup -type=TXT _dmarc.foo.dedyn.io
```

10. Install rootless docker:

```
git clone https://github.com/docker/docker-install /tmp/docker
cd /tmp/docker
./install.sh
dockerd-rootless-setuptool.sh install
```

> 🧐 I tried to run the docker containers without sudo, but that won't work.
> Maybe it's because postfix runs on host as a service, or maybe there is
> another reason as to why. But 'sudo' is required ...

11. Run Virtual Network: `sudo docker network create -d bridge --subnet=10.0.0.0/24 --gateway=10.0.0.1 sl-network`
11. Run Postgres Database: Adjust var PASSWORD (if you change USER/DB too, they then need to be adjusted at other configs too)

> 🧐 random password: `openssl rand -base64 33`

```
sudo docker run -d --name sl-db -e POSTGRES_PASSWORD=mUqIN9KveJeetgRu5USbyPAcXoh0U35D/pdseD+leKtR -e POSTGRES_USER=admin -e POSTGRES_DB=simplelogin \
-p 127.0.0.1:5432:5432 -v $(pwd)/sl/db:/var/lib/postgresql/data --restart always --network="sl-network" postgres:12.1
```

13. Test if you can login to db: `sudo docker exec -it sl-db psql -U admin simplelogin`
13. Install postfix: `apt install postfix postfix-pgsql -y`
    * Internet Site: 'sl.foo.dyndns.io' / 'sl.mydomain.com' (should be automatically correct)
13. Replace Content: `sudo vi /etc/postfix/main.cf` and adjust domain names to yours

```
# POSTFIX config file, adapted for SimpleLogin
smtpd_banner = $myhostname ESMTP $mail_name (Ubuntu)
biff = no

# appending .domain is the MUA's job.
append_dot_mydomain = no

# Uncomment the next line to generate "delayed mail" warnings
#delay_warning_time = 4h

readme_directory = no

# See http://www.postfix.org/COMPATIBILITY_README.html -- default to 2 on
# fresh installs.
compatibility_level = 2

# TLS parameters
smtpd_tls_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
smtpd_tls_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
smtp_tls_security_level = may
smtpd_tls_security_level = may

# See /usr/share/doc/postfix/TLS_README.gz in the postfix-doc package for
# information on enabling SSL in the smtp client.

alias_maps = hash:/etc/aliases
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 10.0.0.0/24

# Set your domain here
mydestination =
myhostname = sl.foo.dedyn.io
mydomain = foo.dedyn.io
myorigin = foo.dedyn.io

relay_domains = pgsql:/etc/postfix/pgsql-relay-domains.cf
transport_maps = pgsql:/etc/postfix/pgsql-transport-maps.cf

# HELO restrictions
smtpd_delay_reject = yes
smtpd_helo_required = yes
smtpd_helo_restrictions =
    permit_mynetworks,
    reject_non_fqdn_helo_hostname,
    reject_invalid_helo_hostname,
    permit

# Sender restrictions:
smtpd_sender_restrictions =
    permit_mynetworks,
    reject_non_fqdn_sender,
    reject_unknown_sender_domain,
    permit

# Recipient restrictions:
smtpd_recipient_restrictions =
   reject_unauth_pipelining,
   reject_non_fqdn_recipient,
   reject_unknown_recipient_domain,
   permit_mynetworks,
   reject_unauth_destination,
   reject_rbl_client zen.spamhaus.org=127.0.0.[2..11],
   reject_rbl_client bl.spamcop.net=127.0.0.2,
   permit
```

16. Add content: `sudo vi /etc/postfix/pgsql-relay-domains.cf`

> 🧐 Adjust to your db credentials and your domain name

```
# postgres config
hosts = localhost
user = admin
password = mUqIN9KveJeetgRu5USbyPAcXoh0U35D/pdseD+leKtR
dbname = simplelogin

# forward to smtp:127.0.0.1:20381 for custom domain AND email domain
query = SELECT 'smtp:127.0.0.1:20381' FROM custom_domain WHERE domain = '%s' AND verified=true
    UNION SELECT 'smtp:127.0.0.1:20381' FROM public_domain WHERE domain = '%s'
    UNION SELECT 'smtp:127.0.0.1:20381' WHERE '%s' = 'foo.dedyn.io' LIMIT 1;
```

17. Add content `sudo vi /etc/postfix/pgsql-transport-maps.cf`

> 🧐 Adjust to your db credentials and your domain name

```
# postgres config
hosts = localhost
user = admin
password = mUqIN9KveJeetgRu5USbyPAcXoh0U35D/pdseD+leKtR
dbname = simplelogin

# forward to smtp:127.0.0.1:20381 for custom domain AND email domain
query = SELECT 'smtp:127.0.0.1:20381' FROM custom_domain WHERE domain = '%s' AND verified=true
    UNION SELECT 'smtp:127.0.0.1:20381' FROM public_domain WHERE domain = '%s'
    UNION SELECT 'smtp:127.0.0.1:20381' WHERE '%s' = 'foo.dedyn.io' LIMIT 1;
```

18. Restart postfix: `sudo systemctl restart postfix`
18. Create sl env vars: `vi ~/simplelogin.env`

> 🧐 Adjust URL, DOMAINS, DB_URI, FLASK_SECRET with `openssl rand -hex 30` output

```
# WebApp URL
URL=http://sl.foo.dedyn.io

# domain used to create alias
EMAIL_DOMAIN=foo.dedyn.io

# transactional email is sent from this email address
SUPPORT_EMAIL=support@foo.dedyn.io

# custom domain needs to point to these MX servers
EMAIL_SERVERS_WITH_PRIORITY=[(10, "sl.foo.dedyn.io.")]

# By default, new aliases must end with ".{random_word}". This is to avoid a person taking all "nice" aliases.
# this option doesn't make sense in self-hosted. Set this variable to disable this option.
DISABLE_ALIAS_SUFFIX=1

# the DKIM private key used to compute DKIM-Signature
DKIM_PRIVATE_KEY_PATH=/dkim.key

# DB Connection
DB_URI=postgresql://admin:mUqIN9KveJeetgRu5USbyPAcXoh0U35D/pdseD+leKtR@sl-db:5432/simplelogin

FLASK_SECRET=pls create a pw

GNUPGHOME=/sl/pgp

LOCAL_FILE_UPLOAD=1

POSTFIX_SERVER=10.0.0.1
```

20. Migrate db:

> 🧐 Run this command from your HOME dir

```
sudo docker run --rm --name sl-migration -v $(pwd)/sl:/sl -v $(pwd)/sl/upload:/code/static/upload -v $(pwd)/dkim.key:/dkim.key \
-v $(pwd)/dkim.pub.key:/dkim.pub.key -v $(pwd)/simplelogin.env:/code/.env --network="sl-network" simplelogin/app:4.6.5-beta alembic upgrade head
```

21. Initialize Simple Login:

> 🧐 Run this command from your HOME dir

```
sudo docker run --rm --name sl-init -v $(pwd)/sl:/sl -v $(pwd)/simplelogin.env:/code/.env -v $(pwd)/dkim.key:/dkim.key -v $(pwd)/dkim.pub.key:/dkim.pub.key \
--network="sl-network" simplelogin/app:4.6.5-beta python init_app.py
```

22. Run webapp:

> 🧐 Run this command from your HOME dir

```
sudo docker run -d --name sl-app -v $(pwd)/sl:/sl -v $(pwd)/sl/upload:/code/static/upload -v $(pwd)/simplelogin.env:/code/.env -v $(pwd)/dkim.key:/dkim.key \
-v $(pwd)/dkim.pub.key:/dkim.pub.key -p 127.0.0.1:7777:7777 --restart always --network="sl-network" simplelogin/app:4.6.5-beta
```

23. Run Email Handler:

```
sudo docker run -d --name sl-email -v $(pwd)/sl:/sl -v $(pwd)/sl/upload:/code/static/upload -v $(pwd)/simplelogin.env:/code/.env -v $(pwd)/dkim.key:/dkim.key \
-v $(pwd)/dkim.pub.key:/dkim.pub.key -p 127.0.0.1:20381:20381 --restart always --network="sl-network" simplelogin/app:4.6.5-beta python email_handler.py
```

24. Run job runner

```
sudo docker run -d --name sl-job-runner -v $(pwd)/sl:/sl -v $(pwd)/sl/upload:/code/static/upload -v $(pwd)/simplelogin.env:/code/.env \
-v $(pwd)/dkim.key:/dkim.key -v $(pwd)/dkim.pub.key:/dkim.pub.key --restart always --network="sl-network" simplelogin/app:4.6.5-beta python job_runner.py
```

25. Install nginx and certbot: `apt install nginx python3-certbot-nginx`
25. Create Proxy: `sudo vi /etc/nginx/sites-available/simplelogin`

```
server {
    server_name  sl.foo.dedyn.io;

    location / {
        proxy_pass              http://localhost:7777;
    }
}
```

27. Symlink to enable: `sudo ln -s /etc/nginx/sites-available/simplelogin /etc/nginx/sites-enabled`
27. Reload service: `sudo systemctl reload nginx`
27. Get SSL cert: `sudo certbot --nginx`
27. Adjust two Lines with your domain and path from certbot output: `sudo vi /etc/postfix/main.cf`

```
smtpd_tls_cert_file=/etc/letsencrypt/live/sl.foo.dedyn.io/fullchain.pem
smtpd_tls_key_file=/etc/letsencrypt/live/sl.foo.dedyn.io/privkey.pem
```

31. Restart postfix: `sudo systemctl restart postfix`
31. Update SL env var: `vi ~/simplelogin.env` (https instead of http)

```
URL=https://sl.foo.dedyn.io
```

33. Restart all docker containers: `sudo docker restart sl-db sl-app sl-email sl-job-runner`
33. Enable HSTS: `sudo vi /etc/nginx/sites-available/simplelogin` (add this line under server_name)

```
add_header Strict-Transport-Security "max-age: 31536000; includeSubDomains" always;
```

35. Restart nginx: `sudo systemctl restart nginx`
35. Login to SL on browser and create an account: `https://sl.foo.dedyn.io`
35. Enable 2FA
35. Make account premium: `sudo docker exec -it sl-db psql -U admin simplelogin`

```
UPDATE users SET lifetime = TRUE;
```

39. Disable new acc registrations: `vi ~/simplelogin.env`

```
DISABLE_REGISTRATION=1
DISABLE_ONBOARDING=true
```

40. restart app: `sudo docker restart sl-app`

## Mobile App

1. Create API Key for SL App
1. Login with API URL is 'https://sl.foo.dedyn.io' and PW is the API key

## Create backup

1. stop db: `sudo docker stop sl-db`
1. create backup: `sudo tar -czf db.tar.gz db/`
1. run app again: `sudo docker restart sl-db`

## Update SL

* [20240204194240](/20240204194240/) Server Service: Update Simple Login

Related:

* <https://github.com/simple-login/app>
* <https://github.com/simple-login/app/blob/master/docs/ssl.md>
* [20240107205508](/20240107205508/) Server Setup: Add domain name to your Server

Tags:

    #linux #server #email #alias #docker #sysadmin
