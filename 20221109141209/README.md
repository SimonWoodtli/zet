# Publish an Image to hub.docker.com Registry

1. create an account if you don't already have one
2. login to website and click 'Create a Repository' (can this be done with CLI?)
3. login CLI: `docker login`
4. command structure tag image: `docker tag containerName:tagVersion dockerHubUserName/containerNameToBePublished:tagVersion`
  1. tag the image example: `docker tag example_image:latest simonwoodtli/example_image:latest`
> 🧐 Of course the example_image needs to be an already locally existing/created image.

5. command structure to publish image to any registry: `docker push registry.example.com/registryUserName/example_image:tagVersion`
  1. publish image to hub.docker.com (default) example: `docker push simonwoodtli/example_image:latest`

Tags:

    #linux #docker #sysadmin #cloud #devops #cloudNative
