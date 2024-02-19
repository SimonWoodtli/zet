# systemd timer vs cron/anacron vs at/batch when to use which?

* sytemd timers: Scheduled guaranteed periodic jobs
* anacron: Scheduled guaranteed periodic jobs
* cron: Scheduled periodic jobs
* at: Scheduled one-time jobs
* batch: Server-load oriented one-time jobs (as apposed to time/date)

## Systemd Timers vs. Anacron explained

> ⚠️ IMPORTANT: Don't enable or start the systemd service unit. Enable the timer
> unit it will trigger/manage the corresponding service unit

The real comparison is with Anacron as it's the only periodic job scheduler
which is guaranteed too (if it can't run at the given time/date it will do so on
next boot up).

Anacron Systemd Timer Comparison: (5 main differences)

1. Who runs it?
* Anacron runs by default as root and with bourne shell (sh). It can be be setup for users but is a bit annoying.
* Can be setup as system-wide root timer units and user time units. It can run any script you want sh,bash,python.
2. How to setup?
* Easy setup: If you are fine with a sh script and a root running process it's very simple to add an anacron job (just put script it into /etc/cron.weekly)
* Setting up systemd timers is more involved it requires three files: service unit files that defines script to be triggered, the script, timer unit file to trigger the service.
3. Which machines/systems support it?
* Anacron runs on any UNIX system
* Systemd Timers run only if the systemd init system is present
4. How to monitor and log your jobs?
* Anacron is a subcmd/system of cron and runs via the crond.service daemon. So logging gets a bit messy.
    * `journalctl | grep CRON`
* Systemd Timers can be monitored/logged easily:
    * Job status: `systemctl status your-timer` or all: `systemctl --type=timer status`
    * Timer status: `systemctl list-timers`
    * `journalctl | grep *.timer`
    * Can be monitored with `Icinga`
5. Cross machine compatibility?
* Anacron/Cron are less likely to work on multiple machines without modifying them because of paths etc.
* Systemd Timers have a consistent run time env. in case you use it on multiple machines

Conclusion:

* Anacron: Best used for less stable things that might change again or get deleted in the future (project related data like project backups)
* Systemd Timer: Best used for stable things such as distro related management (things that are unlikely to change in the future/long term)
    * Or if you need more advanced triggers such as event-based triggers
* Use Case: On laptops/desktops which are not always turned on to have guaranteed execution of your scripts. (If it's a server you can use cronjobs)

## Cron explained: Scheduled periodic jobs

* best used on servers or on jobs that don't matter if they get skipped
* regular user: `crontab -e` and root user: `sudo crontab -e` creates a file in /var/spool/cron/{xnasero,root}
* not specific to any user (you can set user) and system-wide: /etc/crontab
* simple to setup jobs

## At explained: scheduled one-time jobs at specific time

* used for stuff that you run today/tomorrow but not periodically and is time-sensitive.
* used for stuff that does not run periodically just a one time thing -> patching a  program, upgrading firmware etc.

## Batch explained: scheduled one-time jobs at system load avg. drops

* triggers if system load average drops below 0.8
* used for stuff that needs to run at some point but is not time sensitive. So it can wait until the machine has resources available.
* used for stuff that does not run periodically just a one time thing -> patching a  program, upgrading firmware etc.

Related:

* <https://docs.fedoraproject.org/en-US/fedora/latest/system-administrators-guide/monitoring-and-automation/Automating_System_Tasks/>
* <https://www.neteye-blog.com/2022/12/start-using-systemd-timers-instead-of-cron-anacron/>

Tags:

    #linux #jobs #schedule #sync #async #tasks
