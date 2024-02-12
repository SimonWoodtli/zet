# Server Service: Add git server

Adding git server with a remote backup on a VPS (also works on a LAN server)
and cgit as a frontend. We setup a random git repo 'foo' with a barebone
'foo.git' repo on the server for the remote backup. Then we install cgit to
have a frontend to display the repo from a browser.

## 1. Setup Local Machine with a git repo

1. Git init & create dir and commit random file

```
mkdir /tmp/foo && cd /tmp/foo && git init
echo 'hello world' > bla.txt
git add bla.txt
git commit -m "initial commit"
```

2. Add a git remote: (If you don't have a domain just do 'git@ipOfYourVPS'

```
git remote add mygit git@simonwoodtli.com:foo.git`
```

3. Push it: `git push mygit main` (you need to setup git server first to actually push this)

## 2. Setup Git Server/Remote on VPS

> 📝 If you are logged in as root you may ommit sudo but for security practice
> it's better to create a non root user to ssh into server.

1. ssh into your remote server `ssh myUser@ipOfVPS`
1. Create git user (comment is displayed as owner on cgit site): `sudo groupadd git && sudo useradd -m -g git -G users -c "SimonWoodtli" -s /bin/bash git`
1. Create password: `sudo passwd git` (to generate `openssl rand -base64 24`)
1. Copy the ssh pub key:

```
sudo mkdir /home/git/.ssh
sudo cp ~/.ssh/authorized_keys /home/git/.ssh
sudo chown -R git:git .ssh
```

1. On your local machine login as git: `ssh git@IPofVPS`
1. Make main master branch: `git config --global init.defaultBranch main`
1. Create a barebone repo: `git init --bare foo.git`

Now you should be able to `git push` on your local machine. Congrats 🎉

Check if you received the push: `cd ~/foo.git && git log`

## 3. Setup Frontend: cgit

Prerequisites: Have nginx and certbot already installed

* [20240104154437](/20240104154437/) Server Service: Add nginx

1. Install: `apt install cgit fcgiwrap`
1. Edit: `vi /etc/cgitrc`

```
css=/cgit.css
logo=/cgit.svg

virtual-root=/
clone-prefix=http://git.simonwoodtli.com
root-title=Simon's git server
root-desc=A web interface to my git repositories, powered by Cgit
readme=main:README.md

# The location where git repos are stored on the server: Has to be last
scan-path=/home/git/
# You can also list each repo separatly and add more options for each repo
```

3. Create nginx proxy site: `cd /etc/nginx/sites-available/ && vi cgit`
   You only need to adjust server_name,GIT_PROJECT_ROOT and HOME variables

```
server {
    listen 80;
    listen [::]:80;

    root /usr/share/cgit;
    try_files $uri @cgit;
    server_name git.simonwoodtli.com;


    location ~ /.+/(info/refs|git-upload-pack) {
        include             fastcgi_params;
        fastcgi_param       SCRIPT_FILENAME     /usr/lib/git-core/git-http-backend;
        fastcgi_param       PATH_INFO           $uri;
        fastcgi_param       GIT_HTTP_EXPORT_ALL 1;
        fastcgi_param       GIT_PROJECT_ROOT    /home/git/;
        fastcgi_param       HOME                /home/git/;
        fastcgi_pass        unix:/run/fcgiwrap.socket;
    }

    location @cgit {
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME /usr/lib/cgit/cgit.cgi;
        fastcgi_param PATH_INFO $uri;
        fastcgi_param QUERY_STRING $args;
        fastcgi_param HTTP_HOST $server_name;
        fastcgi_pass unix:/run/fcgiwrap.socket;
    }
}
```

4. Enable site: `ln -s $PWD/cgit /etc/nginx/sites-enabled`
4. Restart nginx: `systemctl restart nginx`
4. Create SSL cert: `certbot --nginx`

Related:

* [20210526164049](/20210526164049/) Setup local Git Repo with remote backup
* <https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server>
* <https://wiki.archlinux.org/title/cgit>
* <https://github.com/progit/progit2/releases>

Tags:

      #linux #server #git #remote #repository #source #nginx #cgit
