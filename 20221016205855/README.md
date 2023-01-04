# Docker Basic Commands - Cheatsheet

* if you haven't setup docker.service to autostart at startup: `systemctl
  --user (start|stop|restart) docker.service`
* run docker image with specific port (first num is host, second num is docker
  container): `docker run -t -d -p 80:80 --name myubuntu ubuntu`
* get some cpu/memory usage for docker containers: `docker stats`

## Login Docker: Login Shell, Non Login Shell

* If you want to login to a workspace container use a login shell: `docker exec
  -it <container> bash -c "su - <user>"` , sets \$USER
* If you want to login to a container with a non login shell: `docker exec -it
  <container> bash`, does not set \$USER

> ⚠️  Test if Login Shell: `echo $0` output: -bash, Test if Non Login Shell:
`echo $0` output: bash

## Docker Workflow: Get Image, Run it, Login, Stop

1. pull latest alpine image from hub.docker.com registry: `docker pull alpine`
1. pull older alpine image with a tag: `docker pull alpine:3.14`
2. start an image without tag it runs latest: `docker run -d -t --name
   thisIsAlpine alpine`
2. start an image with tag: `docker run -d -t --name thisIsAlpine alpine:3.14`
3. check if it's running: `docker ps`
4. switch to a running image, add containers shell as argument found in
   'COMMAND' in `docker ps`: `docker exec -it thisIsAlpine /bin/sh`
5. stop a running image: `docker stop thisIsAlpine`
6. start it again: `docker start thisIsAlpine`

## Basics

* list all images: `docker images`
* remove image: `docker rmi <image-id>|<imageName>`
* list all containers: `docker ps -a`
* remove container: `docker rm <container-id>|<containerName>`

Related:

* [20221025194439](/20221025194439/) Docker Templates and Images
* [20221109133025](/20221109133025/) Docker Commit
* [20221109133420](/20221109133420/) Docker Creating Images
* [20221109135030](/20221109135030/) Docker Running Images
* [20221109141209](/20221109141209/) Publish an Image to hub.docker.com Registry

Tags:

    #linux #docker #virtualization #application #container #cheatsheet
