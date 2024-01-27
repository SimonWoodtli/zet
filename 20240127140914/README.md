# Install glibc on Alpine Linux

Many programs run only with glibc and musl won't do. Sometimes you're lucky and
Alpine compiled a separate musl compatible version. If not you have to compile
your programs yourself. Unless you replace musl and use glibc instead. TBH I'm
not sure what kind of bugs I will encounter going down this route but I am
willing to give it a try. I had enough grief with this issue in the past, time
to try something new 👍

I know this is a bit of a hack, but hey you do you. Also as a compromise there
is the `libc6-compat` package. Which I will also have a look at after this
experiment. Also I only really use this on my workstation container to work in.
I would rather not use this on a production container of any sorts.

1. Check ldd: `ldd --version`
1. Install it: (adjust GLIBC_VER var)

```
GLIBC_VER="2.35-r1"
wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub
wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r1/glibc-2.35-r1.apk
apk add glibc-2.35-r1.apk
wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r1/glibc-bin-2.35-r1.apk
wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r1/glibc-i18n-2.35-r1.apk
apk add glibc-bin-2.35-r1.apk glibc-i18n-2.35-r1.apk
/usr/glibc-compat/bin/localedef -i en_US -f UTF-8 en_US.UTF-8
```

3. Update Path: `export PATH=/usr/glibc-compat/bin:$PATH`
3. Check ldd: `ldd --version`

Related:

* <https://wiki.alpinelinux.org/wiki/Running_glibc_programs>
* <https://github.com/sgerrand/alpine-pkg-glibc>
* <https://ariadne.space/2021/08/26/there-is-no-such-thing-as-a-glibc-based-alpine-image/>

Tags:

      #linux #glibc #musl #c #library
