# Server Security: Add fail2ban

In case you don't use best practice V1 or V2 and have an open port having
fail2ban is useful to implement. Although it's main feature is making
brute-force attacks more difficult, and we nullified this attack vector (ssh
key pair auth only). It still helps because you will have fewer attempts on
your server and that results in less logs which is always good. Plus fail2ban
can also be used for web apps with authentication or nginx.

## fail2ban features

> 📚 Fail2ban is a tool designed to protect Linux servers from brute-force
> attacks. Its main feature is to monitor system logs for any malicious
> activity and then execute user-defined actions such as banning the offending
> IP address.

* Helps prevent brute-force password attacks
* Watches logs for authentication failures
* Creates firewall rules to block IP addresses (by default temp. ban)

## fail2ban config

> 🧐 Copy the original configs with a .local suffix and configure them instead.
> They won't get overwritten by future updates of fail2ban

1. Install it and copy configs:

```
sudo apt install fail2ban
cp /etc/fail2ban/fail2ban.{conf,local}
cp /etc/fail2ban/jail.{conf,local}
```

2. edit fail2ban.local:

```
loglevel = INFO
logtarget = /var/log/fail2ban.log
```

3. edit jail.local:
TODO: research sendmail to send status mails from fail2ban

```
banaction = iptables-allports
## How long you stay banned: -1 is perma ban
bantime  = -1
## Time for how many failes are required until ban gets triggered
findtime  = 600
## How many tries within the findtime are neeeded until ban gets triggered
maxretry = 5
## TODO: If you have a static public IP add it to ignoreip to whitelist it
ignoreip = 127.0.0.1/8 ::1
## lookup ip and send banned ip to mail: requires `sendmail`
action = %(action_mwl)s
## Keep mails on server or actually send them to my email? Not sure ...
destemail = root@localhost #inspect mail files on server to see them
sender = fail2ban@simonwoodtli.com
## how to run the daemon
backend = systemd
##TODO in the Jail list at bottom of file activate all the services that your server is running with `enabled = true`
```

4. enable it `systemctl enable fail2ban --now`

> 🧐 To check status of fail2ban: `fail2ban-client status`, and for a specific
> service: `fail2ban-client status sshd` > Make sure jail is setup for all the
> services that you opened the port up for (ideally you used best practices and
> all ports are closed). If you have open ports from a program that must run
> add them to fail2ban!

> 🧐 You can easily test if fail2ban works by using a machine which is not
> whitelisted and keep trying to ssh into your server but on purpose fail. You
> should eventually get banned which results in a connection refused prompt. On
> the servers fail2ban logs you should see that ban `fail2ban-client status
> sshd`. However this will only work if you actually allow password
> authentication for ssh, because you never get a password challenge and you
> never fail the challenge. SO in the logs you won't be able to see anything.

Related:

* <https://www.linode.com/docs/guides/using-fail2ban-to-secure-your-server-a-tutorial/#how-to-install-fail2ban>
* TODO: add link to zet serverinit and zet ssh hardening

Tags:

    #linux #server #sysadmin #vps #security #infosec #networking #firewall
