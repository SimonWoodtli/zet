# Configuring the ntpd Client

## Commands

* Logs: `sudo ntpd -gq`
* Connections: `ntpq -p`
* `ntpq -c`

## Using Server Pools (many servers clustered)

A good NTP server is only as good as its time source. The [NTP Pool
Project][ntp] was created to alleviate the load that was crippling the small
number of NTP servers.The pool directive is aware of round-robin DNS servers
and allows a different IP address on each request. If the connection allows
peers, the pool command will establish peers on its own.

Pool is normally a round robin server setup where any machine in
the "pool" of machines can answer the query.

Fetches time from any server in a pool of servers. (each time you query a different server may response)

edit '/etc/ntp.conf':

```
driftfile /var/lib/ntp/ntp.drift
pool 0.pool.ntp.org
pool 1.pool.ntp.org
pool 2.pool.ntp.org
pool 3.pool.ntp.org
```

## Using a single Server

Fetches time from a single server.

edit '/etc/ntp.conf':

```
server ntp.ubuntu.com
server 172.283.130.222
server 0.pool.ntp.org #In this case I am not sure which server of the pool would get selected. If you `ping 0.pool.ntp.org` you get a different server each time.
```

## Using Peers

Peers work like servers and clients at the same time, so they sync in both ways.

edit '/etc/ntp.conf':

```
peer 192.168.8.20
```

## Systemd NTP Notes

If you are using `timedatectl` instead then look for '/etc/systemd/timesyncd.conf' (not sure if the config is the same to create a custom pool, TODO: research)

* `ntpdc -c peers` to see time difference between local machine and peers
* `timedatectl` on modern systems systemd implementation for NTP

[ntp]: <http://www.pool.ntp.org/en/>

Related:

* https://ubuntu.com/server/docs/use-timedatectl-and-timesyncd
* https://www.digitalocean.com/community/tutorials/how-to-set-up-time-synchronization-on-ubuntu-20-04
* https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/s1-understanding_the_ntpd_configuration_file
* [20240318165205](/20240318165205/) How does the NTP protocol work?
* https://www.redhat.com/sysadmin/chrony-time-services-linux#:~:text=The%20server%20directive%20allows%20you,which%20could%20change%20over%20time.

Tags:

    #linux #sysadmin #time #NTP #timedatectl #ntpq #ntpdc
