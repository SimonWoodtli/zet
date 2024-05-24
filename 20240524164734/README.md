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

```
tar --extract --same-permissions --verbose --file /dev/st0
tar -xpvf /dev/st0
tar  xpvf /dev/st0
```

Tags:

    #linux #terminal #cheatsheet #backup #archive
