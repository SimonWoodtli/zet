# Docker Running Images

The app that actually runs inside your image can be either something simple like a command e.g. `date` or something that continuously runs like an Nginx webserver. The first would mean that when you use `docker run ...` it stops as soon as the `date` command has finished. The latter means if you run the image it keeps running until you use the `docker container stop` command. 

> 🧐 The dockerAppFoo is considered to be the container. Whilst the dockerImage is the image you first have to pull/build and then use it to create a container.

* command: `docker run -it --name dockerAppFoo dockerImage:latest`
* run image as root and share hostdir from host with container (mounted on /mnt/volume): `docker run -u root -v $PWD/hostdir:/mnt/volume -it dockerImage/latest`

> 🧐 useful flag: `--rm` removes the container once it finished running `docker run --rm ...`

Tags:

    #linux #docker #sysadmin #cloud #devops #cloudNative
