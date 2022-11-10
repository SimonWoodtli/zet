# Docker Commit

> ⚠️ Before you use `docker commit` make sure the app is not running so `docker ps` shouldn't list the app you want to commit. If it is still running use `docker container stop`.

Docker commit can be used to save the state of an evolving docker container. For example maybe you adjust things after creating your docker image while running the container. So you might install packages, change config files etc. then you could save the state of the container with `docker commit`. However this will create a Docker parent-child relationship meaning the image that you ran first and started to modify becomes the parent to the newly saved image.

Research: What does the parent-child relationship imply?

Tags:

    #linux #docker #commit #sysadmin #cloud #devops #cloudNative
