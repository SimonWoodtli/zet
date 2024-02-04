# Server Service: Update Simple Login

TODO: Not sure if possible because of the db migration part, but try to create
docker-compose to make this easier maintainable. I think killing the posgres
sl-db container is not required. SO then a docker-compose would be possible but
simple needs to run sl-migration and sl-init once first.

> 🧐 Don't use latest tag, it's over three years old and not a maintained
> version. SL requires to specify the version. Get latest maintained version
> from docker hub link below.

TODO: Go in docker app and try to figure out what app version the docker image
actually runs.  
It's not quite clear what sl-app version the docker images runs. The github
release tags are way further and don't have 4.6.5 but stop at 4.6.2.

1. Adjust to whatever the newest tag version is: `sudo docker pull simplelogin/app:4.6.x-beta`
1. `sudo docker stop sl-email sl-app sl-db sl-job-runner`
1. `sudo docker rm -f sl-email sl-app sl-db sl-job-runner`
1. Run new containers for all parts except creating the network and use the updated tag version.
    * Run in this order: sl-db, sl-migration, sl-init, sl-app, sl-email, sl-job-runner (cmds, see [20240202162304](/20240202162304/))

Related:

* <https://hub.docker.com/r/simplelogin/app/tags>
* [20240202162304](/20240202162304/) Server Service: Add Simple Login
