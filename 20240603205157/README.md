# SELinux context cheatsheet

* list context of files: `ls -Z`
* list context of processes: `ps auZ` or `ps axZ`
* change context of file to etc_t: `chcon -t etc_t somefile`
* apply context type of somefile to foo: `chcon --reference somefile foo`

Tags:

    #linux #sysadmin #security #SELinux #LSM #LFS207
