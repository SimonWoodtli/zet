# Server Service: Add password authentication to nginx site

> 🧐 First time using `htpasswd` you need the -c flag. If you add more users you may ommit it.

FIXME: The current way I configured it works. But when pressing submit it seems to change to http, if that's true it's bad. Investigate this :o

1. Either run `htpasswd -c /etc/nginx/.htpasswd yourUsername` or [generate a .htpasswd file][generate] online and add it manually: `vi /etc/nginx/.htpasswd`
1. E.g with data subdomain site: `vi /etc/nginx/sites-available/data` add auth_basic lines to it:

```
server {
        listen 80;
        listen [::]:80;
        server_name data.simonwoodtli.com;
        root /home/xnasero/www/data;
        index index.html;
        location / {
                autoindex on;
                auth_basic "Restricted Content";
                auth_basic_user_file /etc/nginx/.htpasswd;
        }
}
```

[generate]: <https://www.web2generators.com/apache-tools/htpasswd-generator>

Related:

* <https://www.web2generators.com/apache-tools/htpasswd-generator>
* <https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-nginx-on-ubuntu-20-04>
* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104154437](/20240104154437/) Server Service: Add nginx
* [20240104162118](/20240104162118/) Add https cert for nginx
* [20240107222637](/20240107222637/) Server Service: Add a data server with nginx

Tags:

        #linux #server #security #authentication #webserver #nginx
