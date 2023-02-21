# Docker Creating Images

To create a Docker image either use a Dockerfile or create an image from an already existing Container with `docker commit`.

## From Dockerfile

The `docker build` command builds an image from a Dockerfile.

1. `touch Dockerfile` 
2. edit content:

```
FROM busybox:latest
CMD ["date"]
```

3. create image: `docker build -t example_image .`
4. run image: `docker run -it --name example_app example_image`

> 🧐 If you already build an image and need to rebuild it with updated data, but the Dockerfile is still the same, Docker will use a cached data which in some cases can be useful. However if you want to force Docker to rebuild everything from scratch: `docker build --no-cache -t example_image .`

## From existing Container

Use the `docker commit` command to create an image from the current docker apps state.

1. pull image: `docker pull alpine`
1. run a docker image: `docker run -it --name dockerAppFoo alpine:latest`
1. install all the dependencies for your wanted app and the actual app you want to run.
1. make sure to stop the app: `docker ps` shouldn't list it otherwise: `docker container stop dockerAppFoo`
1. create new docker image: `docker commit dockerAppFoo dockerImageNewFoo:latest`

Related:

* <https://www.pluralsight.com/guides/create-docker-images-docker-hub>
* [20221016205058](/20221016205058/) Docker Setup
* [20221016205855](/20221016205855/) Docker Basic Commands - Cheatsheet
* [20221025194439](/20221025194439/) Docker Templates and Images
* [20221025222121](/20221025222121/) What are Docker Containers?
* [20221026000728](/20221026000728/) 🖼️  Comparison: Docker and VM
* [20221109133025](/20221109133025/) Docker Commit
* [20221109135030](/20221109135030/) Docker Running Images
* [20221109141209](/20221109141209/) Publish an Image to Docker Hub

Tags:

    #linux #docker #sysadmin #cloud #devops #cloudNative
