---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-03.md)
[NEXT](LFS-05.md)

<br>
# LFS: Wrong "lfs" mounting point (earlier OVA file version) 

<br>
## Alas, /dev/sdb2 is mounted on the /lfs (Mea Culpa).

* It should be /mnt/lfs

```
cbkadal@osp:~$ df
Filesystem     1K-blocks    Used Available Use% Mounted on
udev              490184       0    490184   0% /dev
tmpfs             101016    2996     98020   3% /run
/dev/sda2       65543732 4945948  57238552   8% /
tmpfs             505076       0    505076   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs             505076       0    505076   0% /sys/fs/cgroup
/dev/sdb2       65543732   53272  62131228   1% /lfs
tmpfs             101012       0    101012   0% /run/user/1000

cbkadal@osp:~$

```

<br>
## su - root

* unmount /dev/sdb2
* make a  /etc/fstab backup
* edit    /etc/fstab
* remount all (mount -a)
* check it out (df)

```
cbkadal@osp:~$ su -
Password: 

root@osp:~# umount /dev/sdb2

root@osp:~# df
Filesystem     1K-blocks    Used Available Use% Mounted on
udev              490184       0    490184   0% /dev
tmpfs             101016    2996     98020   3% /run
/dev/sda2       65543732 4945952  57238548   8% /
tmpfs             505076       0    505076   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs             505076       0    505076   0% /sys/fs/cgroup
tmpfs             101012       0    101012   0% /run/user/1000

root@osp:~# cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system>                           <mount point>  <type>       <options>     <dump><pass>
# / was on /dev/sda2 during installation
UUID=7acd274a-097d-4910-8c8a-b528d2c4ff9f /               ext4        errors=remount-ro 0  1
# /lfs was on /dev/sdb2 during installation
UUID=0bbf7ab4-6acd-48ea-a298-f23d87a7264a /lfs            ext4        defaults          0  2
# swap was on /dev/sda1 during installation
UUID=65027801-cec4-447d-9056-7f1622527d81 none            swap        sw                0  0
# swap was on /dev/sdb1 during installation
UUID=bf399df9-a433-4bd0-9efd-a3e4386514ec none            swap        sw                0  0
/dev/sr0                                  /media/cdrom0   udf,iso9660 user,noauto       0  0

root@osp:~# cp /etc/fstab /etc/ZOLD.fstab

root@osp:~# vi /etc/fstab
# change /lfs to /mnt/lfs #######################

root@osp:~# cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system>                           <mount point>  <type>       <options>     <dump><pass>
# / was on /dev/sda2 during installation
UUID=7acd274a-097d-4910-8c8a-b528d2c4ff9f /               ext4        errors=remount-ro 0  1
# /lfs was on /dev/sdb2 during installation
UUID=0bbf7ab4-6acd-48ea-a298-f23d87a7264a /mnt/lfs        ext4        defaults          0  2
# swap was on /dev/sda1 during installation
UUID=65027801-cec4-447d-9056-7f1622527d81 none            swap        sw                0  0
# swap was on /dev/sdb1 during installation
UUID=bf399df9-a433-4bd0-9efd-a3e4386514ec none            swap        sw                0  0
/dev/sr0                                  /media/cdrom0   udf,iso9660 user,noauto       0  0

root@osp:~# [ -d /mnt/lfs ] || mkdir -pv /mnt/lfs
mkdir: created directory '/mnt/lfs'

root@osp:~# mount -a

root@osp:~# df
Filesystem     1K-blocks    Used Available Use% Mounted on
udev              490184       0    490184   0% /dev
tmpfs             101016    2996     98020   3% /run
/dev/sda2       65543732 4945960  57238540   8% /
tmpfs             505076       0    505076   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs             505076       0    505076   0% /sys/fs/cgroup
tmpfs             101012       0    101012   0% /run/user/1000
/dev/sdb2       65543732   53272  62131228   1% /mnt/lfs

root@osp:~# 

```


<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-03.md)
[NEXT](LFS-05.md)
<br>

