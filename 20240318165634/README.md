# Configuring the ntpd Client

A good NTP server is only as good as its time source. The [NTP Pool
Project][ntp] was created to alleviate the load that was crippling the small
number of NTP servers.The pool directive is aware of round-robin DNS servers
and allows a different IP address on each request. If the connection allows
peers, the pool command will establish peers on its own.

The classic server directive may be used if desired.

To configure your NTP server to use NTP Pool, edit '/etc/ntp.conf' and add or
edit the following settings:

```
driftfile /var/lib/ntp/ntp.drift
pool 0.pool.ntp.org
pool 1.pool.ntp.org
pool 2.pool.ntp.org
pool 3.pool.ntp.org
```

If you are using `timedatectl` instead then look for '/etc/systemd/timesyncd.conf' (not sure if the config is the same to create a custom pool, TODO: research)

* `ntpdc -c peers` to see time difference between local machine and peers
* `timedatectl` on modern systems systemd implementation for NTP

[ntp]: <http://www.pool.ntp.org/en/>

Tags:

    #linux #sysadmin #time #NTP #timedatectl #ntpq #ntpdc
