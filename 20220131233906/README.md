# Faster bootup (no grub-screen)

$ `sudo vi /etc/default/grub`
```bash
grub timeout set 0
```
$ `sudo update-grub`
