# Anacron Setup

> 🧐 anacron is a subcmd/system of cronjobs and runs on the same crond.service daemon and cronjobs does.

Installing cronie installs both cronjobs and anacronjobs.

## Anacron scripts

* use #!/bin/sh
* use set -e (so errors don’t hang up in the background)
* use >/dev/null 2>&1 redirector for any cmd in the script(so you don’t get any system mail send)
* don’t add a .sh file extension to the filename (only - is allowed)

## Anacron Dirs explained:

* /etc/cron.{daily,hourly,monthly,weekly} put only /bin/sh scripts in here to run
* /etc/cron.hourly doesn’t make much sense at this rate you probably wanna use a cronjob
* /var/spool/anacron is used for the timestamps it generates (don’t put your scripts/cmds in here!)
* /etc/cron.hourly/0anacron is used to generate timestamps and sync
  cron/anacron via crond.service daemon and makes sure it only runs with AC
  power (not running on battery)
* /etc/anacrontab (don’t edit this file), it gives you an overview of the
* /etc/cron.d/ is used to configure the crond daemon #

Related:

* [20240219130552](/20240219130552/) systemd timer vs cron/anacron vs at/batch when to use which?
