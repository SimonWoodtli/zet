# Enable Trim on luks encrypted SSDs

## 1. Get SSD Trim rdy

1. Check if your SSD already can handle trim cmds: `lsblk -D` (If Disc-Gran/Disc-Max are non zero values they can handle trim)

If not working:

> 🧐 If it's a regular SSD without luks you need to check /etc/fstab

2. Edit /etc/crypttab: `vi /etc/crypttab`, Make sure 'nofail,discard' flags are at the end of your SSDs mount cmds. E.g

```
2ndHDD LABEL=crypto-2ndHDD none nofail,discard
```

3. Reboot system and recheck: `lsblk -D`

If not working:

4. Edit the LUKS header with the discard flag: `sudo cryptsetup --perf-no_read_workqueue --perf-no_write_workqueue --allow-discards --persistent refresh <luksContainerName>`
4. Check again: `lsblk -D` and `cryptsetup luksDump  /dev/sda1 | grep Flags`

## 2. Enable Trim

`systemctl enabel fstrim.timer`

Related:

* <https://blog.rabin.io/linux/enable-discard-trim-support-for-luks-encrypted-disk>
* <https://wiki.archlinux.org/title/Dm-crypt/Specialties#Discard/TRIM_support_for_solid_state_drives_(SSD)>

Tags:

    #linux #luks #encryption #defrag #trim #SSD
