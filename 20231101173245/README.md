# Create a LVM container from two files

1. create imagefile:

```
dd if=/dev/zero of=~/imagefile0 bs=200M count=1
dd if=/dev/zero of=~/imagefile1 bs=200M count=1
```

2. assign imagefile to first available loop device:

```
sudo losetup -fP ~/imagefile0
sudo losetup -fP ~/imagefile1
```

3. create partition volume on image file:

> 🧐 First list all loop devices and look for the new ones

```
losetup -a
sudo pvcreate /dev/loopXX
sudo pvcreate /dev/loopXX
```

4. Create volume group and assign partition volumes to it:

```
sudo vgcreate myvg /dev/loopXX /dev/loopXX
```

5. create LVM and assign volume group to it:

```
sudo lvcreate -L 300M -n mylvm myvg
```

6. format the LVM:

```
sudo lvdisplay
sudo mkfs.ext4 /dev/myvg/mylvm
```

6.1 Resize the LVM (for demo):

> Within the allowed LVM size we can adjust the LVM while mounted! Notice
we cannot go over the 400M, because we set the imagefiles each to 200M.

works:

```
df -h
sudo lvresize -r -L 350M /dev/myvg/mylvm
#sudo lvresize -r -L +50M /dev/myvg/mylvm
df -h
```

doesn't work: insufficient space error

```
sudo lvresize -r -L 450M /dev/myvg/mylvm
```

7. mount the LVM:

```
sudo mount /dev/myvg/mylvm /mnt
```

8. Auto mount LVM: add to /etc/fstab

```
/dev/myvg/mylvm /mylvm ext4 defaults 0 0
```

kernel parameter documentation: https://www.kernel.org/doc/Documentation/

Tags:

    #linux #LVM #sysadmin #diskPartitionAlternative
