# Server Service: Add a data server with nginx

1. create dir: `mkdir ~/www/data`
1. crate a testfile: `echo blabla > ~/www/data/foo.txt`
1. create config file: `touch /etc/nginx/sites-available/data`

```
server {
        listen 80;
        listen [::]:80;
        server_name data.simonwoodtli.com;
        root /home/xnasero/www/data;
        index index.html;
        location / {
                autoindex on;
        }
}
```

4. symlink: `ln -s /etc/nginx/sites-available/data /etc/nginx/sites-enabled/`
4. restart nginx: `systemctl restart nginx`

## Next Steps

Add password authentication: [20240108005229](/20240108005229/) Server Service: Add password authentication to nginx site

Related:

* [20240104161046](/20240104161046/) Fresh Server: First Steps
* [20240104154437](/20240104154437/) Server Service: Add nginx
* [20240104162118](/20240104162118/) Add https cert for nginx

Tags:

    #linux #server #webdev #nginx #data
