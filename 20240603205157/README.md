# SELinux cheatsheet

* get status: `getenforce` or `sestatus`

## Context type

* list context of files: `ls -Z`
* list context of processes: `ps auZ` or `ps axZ`
* change context of file to etc_t: `chcon -t etc_t somefile`
* apply context type of somefile to foo: `chcon --reference somefile foo`
* restore context type to parent: `restorecon -Rv /home/user/foo` (useful after moving dir|file)
* force restore context type to parent: `restorecon -RFv /home/user/foo`
* change default context type for a given dir: `semanage fcontext -a -t httpd_sys_content_t /virtualHosts` (newly created dir will use the new context type)

## Policies

SELinux policy behavior can be configured at runtime without rewriting the
policy. This is accomplished by configuring SELinux Booleans, which are policy
parameters that can be enabled and disabled.

* to see booleans: `getsebool`
* to set booleans: `setsebool`
* to set booleans (persistent): `setsebool -P allow_ftpd_anon_write on`
* to see persistent boolean settings: `semanage boolean -l`

## Monitor SELinux Access

If you install the 'setroubleshoot-server' package. It will now log to '/var/log/audit/audit.log' and '/var/log/messages'

* detailed msges: `sealert`

Tags:

    #linux #sysadmin #security #SELinux #LSM #LFS207
