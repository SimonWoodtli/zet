# rsync cheatsheet

> 🧐 A simple (and very effective and very fast) backup strategy is to simply
> duplicate directories or partitions across a network with rsync commands and
> to do so frequently.

rsync (remote synchronize) is used to transfer files across a network (or
between different locations on the same machine), as in: `rsync [options]
sourcefile destinationfile`

* dry run: `rsync -r --dry-run /usr/local /BACKUP/usr`
* some good flags to use: `rsync -avzxiPH --delete-after --exclude=.git/ "$tarball" "$usbStick"`

## Flags explained

* `-–delete-after` is better, because it will first copy all the files to the
  backup place and only when it's finished with that it starts looking if there
  are any files on the backup place that don't exist on the source place and
  then delete them. --delete does both things simultaneously.
* add: -P and -u and -z and -h
* -AX makes sense if it is a backup rsync but for a folder to webserver sync it makes no sense.
* always use -a for backups

-u = only update the files that have changed
-v = verbose
-r = recursive for dir
-P = does 2 things, 1 –partial: allows partial files: if rsync runs
and only uploads/copies half the file and then the internet stopped
working or computer crashes. The partially uploaded file remains on
the receiving computer and next time you run rsync again it will
continue uploading where you left off. 2: –progress => show you
percentage of copy process

-a = archive turns on a lot same as -rlptgoD  idea is to preserve attributes
-i = output a change-summary for all updates
-H = preserve Hardlinks
-E = preserve executability  only useful if -p is not enabled
-A = Preserve ACLs; also implies -p (preserve permissions)
-X = preserve extended attributes
-x = don’t cross filesystem boundaries. my interpretation (needs testing): rsyncs only within the scope of the selected partition. only the partitions involved like rsync ~
-h = Human readable numbers are output instead of bytes
--one-file-system, -x = don’t cross filesystem boundaries
-z = compress file data during transfer
-l = copy symlinks as symlinks

Related:

* https://jumpcloud.com/blog/how-to-use-rsync-remote-backup-linux-system
* https://nadaring.mademoiselleosaki.com/content-https-github.com/danie1k/rsync-backup/blob/master/src/rsync_offsite_backup.sh

Tags:

    #linux #sysadmin #archive #backup #rsync #network #cheatsheet
