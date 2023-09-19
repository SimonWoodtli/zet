# Create and run and nbd server and client on same system

Normally you would setup the server and client on different systems but
for simplicity this is going to run on localhost.

1. install server/client packages:
    1. Fedora: `sudo dnf install nbdkit nbd`
    1. Ubuntu: `sudo dnf install nbd-server nbd-client`
    1. Alpine: `sudo apk add nbd nbd-client`
    1. Or build nbd from [source]
2. create server conf: choose local or global
    1. local: ~/.config/nbd/config
    1. global: /etc/nbd-server/config

```
[generic]
  allowlist = 1
  #change this to ip of server if not localhost
  listenaddr = 0.0.0.0
  port = 10042
#name1 of the exported block device
[foo]
  #by FHS convention you should put the shared block device into /opt
  exportname = /opt/dsk1
  readonly = false
#name2 of the exported block device
[bar]
  exportname = /opt/dsk2
```

3. Server: create two empty files filled with junk (choose the size according to
   your needs)

```
sudo dd if=/dev/zero of=/opt/dsk1 status=progress bs=100M count=5
sudo dd if=/dev/zero of=/opt/dsk2 status=progress bs=1G count=2
```

4. Server: Set permissions so regular user on client can access files:

```
sudo chmod 777 /opt
sudo chown clientuser:clientuser /opt/*
```

5. Server: Run server

```
sudo nbd-server -C ~/.config/nbd/config
```

6. Client: Test if client can connect

```
sudo nbd-client -l 127.0.0.1 -p 10042
```

7. Client: load nbd block devices from kernel module

```
sudo modprobe -i nbd
ls /dev/nbd*
```

8. Client: Connect to shared block devices:

> ⚠ Change localhost to your servers IP if you make a real setup

```
sudo nbd-client -N foo 127.0.0.1 10042 /dev/nbd0
sudo nbd-client -N bar 127.0.0.1 10042 /dev/nbd1
```

9. Client: Format the block devices (partition table, partition, filesystem)

> 🧐 Repeat Step 9 and 10 for the second block device

```
sudo parted -s /dev/nbd0 -- mklabel gpt
sudo parted -s /dev/nbd0 -- mkpart primary ext4 0% 100%
sudo mkfs.ext4 -L nbd-foo /dev/nbd0
```

10. Client: Mount the newly formated filesystem:

```
sudo mount /dev/nbd0 /mnt
```

[source]: <https://github.com/NetworkBlockDevice/nbd>

Tags:

    #linux #sysadmin #network #fileSharing #nbd
