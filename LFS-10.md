---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-08.md)
[NEXT](index.md)

<br>
<span style="color:red; font-weight:bold; font-size:larger;">
It is assumed that you understand how install a Debian VirtualBox Guest.
If you have never installed a VirtualBox Guest before, visit [OSP4DISS](https://osp4diss.vlsm.org/).
</span>

<br>
# LFS: Chapter 7

## Virtual Box Guest LFS-07

* Import LFS-06-part2.ova, rename to LFS-07

<br>
<img src="pictures/LFS-A43.jpg" width="960">

<br>
### INPUT
```
ssh -p 6024 lfs@localhost

```

### OUTPUT
```
rms46@pamulang1:~$ ssh -p 6024 lfs@localhost
lfs@localhost's password:

===== TL;DR =====

lfs@osp:~$ 

```

<br>
### INPUT
```
echo $LFS
su -

```

### OUTPUT
```
lfs:~$ echo $LFS
/mnt/lfs

lfs:~$ su -
Password:

root:~#

```

<br>
# Changing Ownership

### INPUT
```
echo $LFS
chown -R root:root $LFS/{usr,lib,var,etc,bin,sbin,tools}
case $(uname -m) in
  x86_64) chown -R root:root $LFS/lib64 ;;
esac

```

### OUTPUT
```
root:~# chown -R root:root $LFS/{usr,lib,var,etc,bin,sbin,tools}

root:~# case $(uname -m) in
>   x86_64) chown -R root:root $LFS/lib64 ;;
> esac

root:~# 

```

<br>
# Preparing Virtual Kernel File Systems

### INPUT
```
mkdir -pv $LFS/{dev,proc,sys,run}
mknod -m 600 $LFS/dev/console c 5 1
mknod -m 666 $LFS/dev/null c 1 3
mount -v --bind /dev $LFS/dev
mount -v --bind /dev/pts $LFS/dev/pts
mount -vt proc proc $LFS/proc
mount -vt sysfs sysfs $LFS/sys
mount -vt tmpfs tmpfs $LFS/run
if [ -h $LFS/dev/shm ]; then
  mkdir -pv $LFS/$(readlink $LFS/dev/shm)
fi

```

### OUTPUT
```
root:~# mkdir -pv $LFS/{dev,proc,sys,run}
mkdir: created directory '/mnt/lfs/dev'
mkdir: created directory '/mnt/lfs/proc'
mkdir: created directory '/mnt/lfs/sys'
mkdir: created directory '/mnt/lfs/run'

root:~# mknod -m 600 $LFS/dev/console c 5 1

root:~# mknod -m 666 $LFS/dev/null c 1 3

root:~# mount -v --bind /dev $LFS/dev
mount: /dev bound on /mnt/lfs/dev.

root:~# mount -v --bind /dev/pts $LFS/dev/pts
mount: /dev/pts bound on /mnt/lfs/dev/pts.

root:~# mount -vt proc proc $LFS/proc
mount: proc mounted on /mnt/lfs/proc.

root:~# mount -vt sysfs sysfs $LFS/sys
mount: sysfs mounted on /mnt/lfs/sys.

root:~# mount -vt tmpfs tmpfs $LFS/run
mount: tmpfs mounted on /mnt/lfs/run.

root:~# if [ -h $LFS/dev/shm ]; then
>   mkdir -pv $LFS/$(readlink $LFS/dev/shm)
> fi

root:~# 

```

<br>
# Entering the Chroot Environment

<span style="color:red; font-weight:bold; font-size:larger;">
ATTN: CHROOT! Compare File System: "df" before, and "df" after.
</span>


### INPUT
```
df

chroot "$LFS" /usr/bin/env -i   \
    HOME=/root                  \
    TERM="$TERM"                \
    PS1='(lfs chroot) \u:\w\$ ' \
    PATH=/bin:/usr/bin:/sbin:/usr/sbin \
    /bin/bash --login +h

df

```

### OUTPUT
```
root:~# df
Filesystem     1K-blocks    Used Available Use% Mounted on
udev             4020816       0   4020816   0% /dev
tmpfs             807128    8636    798492   2% /run
/dev/sda1       16446332 2724032  12867160  18% /
tmpfs            4035640       0   4035640   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs            4035640       0   4035640   0% /sys/fs/cgroup
/dev/sdb1       32894736 5859592  25341144  19% /mnt/lfs
tmpfs             807128       0    807128   0% /run/user/1001
tmpfs            4035640       0   4035640   0% /mnt/lfs/run

root:~# chroot "$LFS" /usr/bin/env -i   \
>     HOME=/root                  \
>     TERM="$TERM"                \
>     PS1='(lfs chroot) \u:\w\$ ' \
>     PATH=/bin:/usr/bin:/sbin:/usr/sbin \
>     /bin/bash --login +h

(lfs chroot) I have no name!:/# df
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/sdb1       32894736 5859592  25341144  19% /
udev             4020816       0   4020816   0% /dev
tmpfs            4035640       0   4035640   0% /run

(lfs chroot) I have no name!:/# 

```

<br>
# Creating Directories

### INPUT
```
mkdir -pv /{boot,home,mnt,opt,srv}
mkdir -pv /etc/{opt,sysconfig}
mkdir -pv /lib/firmware
mkdir -pv /media/{floppy,cdrom}
mkdir -pv /usr/{,local/}{bin,include,lib,sbin,src}
mkdir -pv /usr/{,local/}share/{color,dict,doc,info,locale,man}
mkdir -pv /usr/{,local/}share/{misc,terminfo,zoneinfo}
mkdir -pv /usr/{,local/}share/man/man{1..8}
mkdir -pv /var/{cache,local,log,mail,opt,spool}
mkdir -pv /var/lib/{color,misc,locate}
ln -sfv /run /var/run
ln -sfv /run/lock /var/lock
install -dv -m 0750 /root
install -dv -m 1777 /tmp /var/tmp

```

### OUTPUT
```
(lfs chroot) I have no name!:/# mkdir -pv /{boot,home,mnt,opt,srv}
mkdir: created directory '/boot'
mkdir: created directory '/home'
mkdir: created directory '/mnt'
mkdir: created directory '/opt'
mkdir: created directory '/srv'

(lfs chroot) I have no name!:/# mkdir -pv /etc/{opt,sysconfig}
mkdir: created directory '/etc/opt'
mkdir: created directory '/etc/sysconfig'

(lfs chroot) I have no name!:/# mkdir -pv /lib/firmware
mkdir: created directory '/lib/firmware'

(lfs chroot) I have no name!:/# mkdir -pv /media/{floppy,cdrom}
mkdir: created directory '/media'
mkdir: created directory '/media/floppy'
mkdir: created directory '/media/cdrom'

(lfs chroot) I have no name!:/# mkdir -pv /usr/{,local/}{bin,include,lib,sbin,src}
mkdir: created directory '/usr/src'
mkdir: created directory '/usr/local'
mkdir: created directory '/usr/local/bin'
mkdir: created directory '/usr/local/include'
mkdir: created directory '/usr/local/lib'
mkdir: created directory '/usr/local/sbin'
mkdir: created directory '/usr/local/src'

(lfs chroot) I have no name!:/# mkdir -pv /usr/{,local/}share/{color,dict,doc,info,locale,man}
mkdir: created directory '/usr/share/color'
mkdir: created directory '/usr/share/dict'
mkdir: created directory '/usr/local/share'
mkdir: created directory '/usr/local/share/color'
mkdir: created directory '/usr/local/share/dict'
mkdir: created directory '/usr/local/share/doc'
mkdir: created directory '/usr/local/share/info'
mkdir: created directory '/usr/local/share/locale'
mkdir: created directory '/usr/local/share/man'

(lfs chroot) I have no name!:/# mkdir -pv /usr/{,local/}share/{misc,terminfo,zoneinfo}
mkdir: created directory '/usr/share/zoneinfo'
mkdir: created directory '/usr/local/share/misc'
mkdir: created directory '/usr/local/share/terminfo'
mkdir: created directory '/usr/local/share/zoneinfo'

(lfs chroot) I have no name!:/# mkdir -pv /usr/{,local/}share/man/man{1..8}
mkdir: created directory '/usr/share/man/man2'
mkdir: created directory '/usr/share/man/man6'
mkdir: created directory '/usr/local/share/man/man1'
mkdir: created directory '/usr/local/share/man/man2'
mkdir: created directory '/usr/local/share/man/man3'
mkdir: created directory '/usr/local/share/man/man4'
mkdir: created directory '/usr/local/share/man/man5'
mkdir: created directory '/usr/local/share/man/man6'
mkdir: created directory '/usr/local/share/man/man7'
mkdir: created directory '/usr/local/share/man/man8'

(lfs chroot) I have no name!:/# mkdir -pv /var/{cache,local,log,mail,opt,spool}
mkdir: created directory '/var/cache'
mkdir: created directory '/var/local'
mkdir: created directory '/var/log'
mkdir: created directory '/var/mail'
mkdir: created directory '/var/opt'
mkdir: created directory '/var/spool'

(lfs chroot) I have no name!:/# mkdir -pv /var/lib/{color,misc,locate}
mkdir: created directory '/var/lib/color'
mkdir: created directory '/var/lib/misc'
mkdir: created directory '/var/lib/locate'

(lfs chroot) I have no name!:/# ln -sfv /run /var/run
'/var/run' -> '/run'

(lfs chroot) I have no name!:/# ln -sfv /run/lock /var/lock
'/var/lock' -> '/run/lock'

(lfs chroot) I have no name!:/# install -dv -m 0750 /root
install: creating directory '/root'

(lfs chroot) I have no name!:/# install -dv -m 1777 /tmp /var/tmp
install: creating directory '/tmp'
install: creating directory '/var/tmp'

(lfs chroot) I have no name!:/# 

```

<br>
# Creating Essential Files and Symlinks

### INPUT
```
ln -sv /proc/self/mounts /etc/mtab
echo "127.0.0.1 localhost $(hostname)" > /etc/hosts
cat > /etc/passwd << "EOF"
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/dev/null:/bin/false
daemon:x:6:6:Daemon User:/dev/null:/bin/false
messagebus:x:18:18:D-Bus Message Daemon User:/var/run/dbus:/bin/false
nobody:x:99:99:Unprivileged User:/dev/null:/bin/false
EOF

cat > /etc/group << "EOF"
root:x:0:
bin:x:1:daemon
sys:x:2:
kmem:x:3:
tape:x:4:
tty:x:5:
daemon:x:6:
floppy:x:7:
disk:x:8:
lp:x:9:
dialout:x:10:
audio:x:11:
video:x:12:
utmp:x:13:
usb:x:14:
cdrom:x:15:
adm:x:16:
messagebus:x:18:
input:x:24:
mail:x:34:
kvm:x:61:
wheel:x:97:
nogroup:x:99:
users:x:999:
EOF

echo "tester:x:$(ls -n $(tty) | cut -d" " -f3):101::/home/tester:/bin/bash" >> /etc/passwd
echo "tester:x:101:" >> /etc/group
install -o tester -d /home/tester
exec /bin/bash --login +h
touch /var/log/{btmp,lastlog,faillog,wtmp}
chgrp -v utmp /var/log/lastlog
chmod -v 664  /var/log/lastlog
chmod -v 600  /var/log/btmp

```

### OUTPUT
```
(lfs chroot) I have no name!:/# ln -sv /proc/self/mounts /etc/mtab
'/etc/mtab' -> '/proc/self/mounts'

(lfs chroot) I have no name!:/# echo "127.0.0.1 localhost $(hostname)" > /etc/hosts

(lfs chroot) I have no name!:/# cat > /etc/passwd << "EOF"
> root:x:0:0:root:/root:/bin/bash
> bin:x:1:1:bin:/dev/null:/bin/false
> daemon:x:6:6:Daemon User:/dev/null:/bin/false
> messagebus:x:18:18:D-Bus Message Daemon User:/var/run/dbus:/bin/false
> nobody:x:99:99:Unprivileged User:/dev/null:/bin/false
> EOF

(lfs chroot) I have no name!:/# cat > /etc/group << "EOF"
> root:x:0:
> bin:x:1:daemon

===== TL;DR =====

> nogroup:x:99:
> users:x:999:
> EOF

(lfs chroot) I have no name!:/# echo "tester:x:$(ls -n $(tty) | cut -d" " -f3):101::/home/tester:/bin/bash" >> /etc/passwd

(lfs chroot) I have no name!:/# echo "tester:x:101:" >> /etc/group

(lfs chroot) I have no name!:/# install -o tester -d /home/tester

(lfs chroot) I have no name!:/# exec /bin/bash --login +h

(lfs chroot) root:/# touch /var/log/{btmp,lastlog,faillog,wtmp}

(lfs chroot) root:/# chgrp -v utmp /var/log/lastlog
changed group of '/var/log/lastlog' from root to utmp

(lfs chroot) root:/# chmod -v 664  /var/log/lastlog
mode of '/var/log/lastlog' changed from 0644 (rw-r--r--) to 0664 (rw-rw-r--)

(lfs chroot) root:/# chmod -v 600  /var/log/btmp
mode of '/var/log/btmp' changed from 0644 (rw-r--r--) to 0600 (rw-------)

(lfs chroot) root:/# 

```

<br>
# Libstdc++ from GCC-10.2.0, Pass 2

### INPUT
```
exit

```

### OUTPUT
```

```

<br>
### INPUT
```
tar xf gcc-10.2.0.tar.xz
cd gcc-10.2.0/
ln -s gthr-posix.h libgcc/gthr-default.h
mkdir -v build
cd       build
../libstdc++-v3/configure            \
    CXXFLAGS="-g -O2 -D_GNU_SOURCE"  \
    --prefix=/usr                    \
    --disable-multilib               \
    --disable-nls                    \
    --host=$(uname -m)-lfs-linux-gnu \
    --disable-libstdcxx-pch

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf gcc-10.2.0.tar.xz

(lfs chroot) root:/sources# cd gcc-10.2.0/

(lfs chroot) root:/sources/gcc-10.2.0# ln -s gthr-posix.h libgcc/gthr-default.h

(lfs chroot) root:/sources/gcc-10.2.0# mkdir -v build
mkdir: created directory 'build'

(lfs chroot) root:/sources/gcc-10.2.0# cd       build

(lfs chroot) root:/sources/gcc-10.2.0/build# ../libstdc++-v3/configure            \
>     CXXFLAGS="-g -O2 -D_GNU_SOURCE"  \
>     --prefix=/usr                    \
>     --disable-multilib               \

===== TL;DR =====

    -e 's/\([ABCDEFGHIJKLMNOPQRSTUVWXYZ_]*USE_WEAK\)/_GLIBCXX_\1/g' \
    -e 's,^#include "\(.*\)",#include <bits/\1>,g' \
    < /sources/gcc-10.2.0/libstdc++-v3/../libgcc/gthr-posix.h > x86_64-lfs-linux-gnu/bits/gthr-default.h
(lfs chroot) root:/sources/gcc-10.2.0/build# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/gcc-10.2.0/build# time make
make "AR_FLAGS=" "CC_FOR_BUILD=" "CC_FOR_TARGET=" "CFLAGS=-g -O2" "CXXFLAGS=-g -O2 -D_GNU_SOURCE"

===== TL;DR =====

real	1m20.735s
user	1m13.227s
sys	0m5.271s

(lfs chroot) root:/sources/gcc-10.2.0/build# 

```

<br>
### INPUT
```
time make install
cd ../../
rm -rf gcc-10.2.0/

```

### OUTPUT
```
(lfs chroot) root:/sources/gcc-10.2.0/build# time make install
Making install in include
make[1]: Entering directory '/sources/gcc-10.2.0/build/include'
make[2]: Entering directory '/sources/gcc-10.2.0/build/include'

===== TL;DR =====

real	0m2.715s
user	0m1.763s
sys	0m0.709s

(lfs chroot) root:/sources/gcc-10.2.0/build# cd ../..

(lfs chroot) root:/sources# rm -rf gcc-10.2.0/

(lfs chroot) root:/sources# 

```

<br>
# Gettext-0.21

### INPUT
```
tar xf gettext-0.21.tar.xz
cd gettext-0.21/
./configure --disable-shared

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf gettext-0.21.tar.xz

(lfs chroot) root:/sources# cd gettext-0.21/

(lfs chroot) root:/sources/gettext-0.21# ./configure --disable-shared
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: creating installpaths
config.status: creating po/Makefile
config.status: executing po-directories commands

(lfs chroot) root:/sources/gettext-0.21# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/gettext-0.21# time make

===== TL;DR =====

real	2m40.902s
user	2m27.278s
sys	0m16.910s

(lfs chroot) root:/sources/gettext-0.21# 

```

<br>
### INPUT
```
cp -v gettext-tools/src/{msgfmt,msgmerge,xgettext} /usr/bin
cd ../
rm -rf gettext-0.21/

```

### OUTPUT
```
(lfs chroot) root:/sources/gettext-0.21# cp -v gettext-tools/src/{msgfmt,msgmerge,xgettext} /usr/bin
'gettext-tools/src/msgfmt' -> '/usr/bin/msgfmt'
'gettext-tools/src/msgmerge' -> '/usr/bin/msgmerge'
'gettext-tools/src/xgettext' -> '/usr/bin/xgettext'

(lfs chroot) root:/sources/gettext-0.21# cd ../

(lfs chroot) root:/sources# rm -rf gettext-0.21

(lfs chroot) root:/sources# 

```

<br>
# Bison-3.7.1

### INPUT
```

```

### OUTPUT
```

===== TL;DR =====
```

<br>
### INPUT
```

```

### OUTPUT
```

===== TL;DR =====
```

<br>
### INPUT
```

```

### OUTPUT
```

===== TL;DR =====
```











<hr>
<hr>
<hr>

[](X 13 XXXXXX)
<br>
### INPUT
```

```

### OUTPUT
```

===== TL;DR =====
```

<hr>
<hr>
<hr>

```
su -
```

```
cbkadal@osp:~$ su -
Password:

root:~#

```

```
shutdown -h now

```

```
root:~# shutdown -h now
Connection to localhost closed by remote host.
Connection to localhost closed.

rms46@pamulang1:~$

```

* Back to "pamulang1" host

* Create LFS-07.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-08.md)
[NEXT](index.md)
<br>

