# Docker Running Images

The app that actually runs inside your image can be either something simple like a command e.g. `date` or something that continuously runs like an Nginx webserver. The first would mean that when you use `docker run ...` it stops as soon as the `date` command has finished. The latter means if you run the image it keeps running until you use the `docker container stop` command. 

> 🧐 The dockerAppFoo is considered to be the container. Whilst the dockerImage is the image you first have to pull/build and then use it to create a container.

* command: `docker run -it --name dockerAppFoo dockerImage:latest`

Tags:

    #linux #docker #sysadmin #cloud #devops #cloudNative  
