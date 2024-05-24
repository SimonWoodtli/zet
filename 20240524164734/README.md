# tar cheatsheet

> 🧐 `tar` may be used with or without single dash

* list content of archive: `tar --list --file /dev/st0`
* list content of archive: `tar -tf /dev/st0`
* verify archive: `tar --compare --verbose --file /dev/st0`
* verify archive: `tar -dvf /dev/st0`

## Archive

* incremental archive (only updates new files within FS that are not yet in older archive, or modified after): `tar --create --newer '2011-12-1' -vzf backup1.tgz /var/tmp`
* incremental archive: `tar --create --after-date '2011-12-1' -vzf backup1.tgz /var/tmp`

* create archive: `tar cvf  /dev/st0 /root`
* create archive: `tar -cvf /dev/st0 /root`
* create multi volume archive: `tar -cMf /dev/st0 /root` (You will be prompted to put the next tape when needed.)

## Extract

> 🧐 The -p or --same-permissions options ensures files are restored with their
> original permissions.

Modern versions of `tar` know which compression is used. So when extracting you don't need to specify the used compression as a flag.

```
tar --extract --same-permissions --verbose --file /dev/st0
tar -xpvf /dev/st0
tar  xpvf /dev/st0
```

## Compression

> 🧐 It is often desired to compress files to save disk space and/or network
> transmission time, especially since modern machines will often find the
> compress -> transmit -> decompress cycle faster than just transmitting (or
> copying) an uncompressed file.

> 🧐 Obviously, it is not worth using these methods on archives whose component
> files are already compressed, such as .jpg images, or .pdf files, etc.

* gzip: Uses Lempel-Ziv Coding (LZ77) and produces .gz files.
* bzip2: Uses Burrows-Wheeler block sorting text compression algorithm and Huffman coding, and produces .bz2 files.
* xz: Produces .xz files and also supports legacy .lzma format.

archive+compress: (same as `tar cvf source.tar source ; gzip -v source.tar`)

```
tar zcvf source.tar.gz source
tar jcvf source.tar.bz2 source
tar Jcvf source.tar.xz source
```

Tags:

    #linux #terminal #cheatsheet #backup #archive
