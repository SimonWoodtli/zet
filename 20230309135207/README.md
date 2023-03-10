# Distrobox cheatsheet

* create container: `distrobox create -n <container> -i <image>`
* run container: `distrobox enter <container>`
* stop running container: `distrobox stop <container>`
* clone container: `distrobox -c <container> -n <clonedContainer>`
* rm container: `distrobox rm <container>`
* list all containers: `distrobox list`
* upgrade all packages in all containers: `distrobox upgrade -a`
* run pkg/app within host from a container: `distrobox enter -n <container> -- /usr/bin/foo`
* export app to host: `distrobox-export --app <pkg>`
* export binary to host: `distrobox-export --bin /usr/bin/foo --export-path ~/.local/bin`

Related:

* <https://github.com/89luca89/distrobox/blob/main/docs/compatibility.md>
* <https://github.com/89luca89/distrobox/blob/main/docs/compatibility.md#containers-distros>
* <https://distrobox.privatedns.org/useful_tips.html#using-distrobox-as-main-cli>
* <https://github.com/89luca89/distrobox/blob/main/distrobox-create>
