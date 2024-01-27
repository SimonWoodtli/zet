# Install nodejs on Alpine Linux

My nodejs install script won't work as it fetches from nodejs directly with the
official builds which are compiled with glibc. But node also releases an
unofficial build. Get it with `curl -LJO https://unofficial-builds.nodejs.org/download/release/v20.10.0/node-v20.10.0-linux-x64-musl.tar.gz`

## Via apk

You won't get the latest nodejs but it's something.

`apk add nodejs`

## Via asdf

If you try to do a regular asdf plugin install it won't work. Because the
nodejs you will get ins compiled with glibc. As a workaround you have to tell
asdf to build it then it should automatically realize musl and build it
correctly.

> 📝 Be prepared this might take a while to compile, use parallel to speed it up CONCURRENCY var.

1. Install dependencies: `apk add openssl linux-headers`
1. Add plugin: `asdf plugin-add nodejs`
1. Compile latest: `ASDF_CONCURRENCY=8 ASDF_NODEJS_VERBOSE_INSTALL=1 ASDF_NODEJS_FORCE_COMPILE=1 asdf install nodejs latest`

## Via docker

* <https://hub.docker.com/_/node/>

Related:

* <https://github.com/asdf-vm/asdf-nodejs/>
* <https://github.com/asdf-vm/asdf-nodejs/issues/190>
* <https://github.com/nodejs/node/blob/main/BUILDING.md#building-nodejs-on-supported-platforms>

Tags:

    #linux #nodejs #alpine #asdf #docker #compile
