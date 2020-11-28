---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-05.md)
[NEXT](LFS-07.md)

<br>
<span style="color:red; font-weight:bold; font-size:larger;">
It is assumed that you understand how install a Debian VirtualBox Guest.
If you have never installed a VirtualBox Guest before, visit [OSP4DISS](https://osp4diss.vlsm.org/).
</span>

<br>
# LFS: Chapter 4

## Virtual Box Guest LFS-04

* Import LFS-03.ova, rename to LFS-04

<br>
<img src="pictures/LFS-A38.jpg" width="960">

<br>
### INPUT
```
ssh -p 6024 cbkadal@localhost

```

### OUTPUT
```
rms46@pamulang1:~$ ssh -p 6024 cbkadal@localhost
cbkadal@localhost's password:

===== TL;DR =====

cbkadal@osp:~$ 

```

<br>
### INPUT
```
su -

```

### OUTPUT
```
cbkadal@osp:~$ su -
Password:

root:~#

```

<br>
### INPUT
```
echo $LFS
mkdir -pv $LFS/{bin,etc,lib,sbin,usr,var}
case $(uname -m) in
  x86_64) mkdir -pv $LFS/lib64 ;;
esac
mkdir -pv $LFS/tools
groupadd lfs
useradd -s /bin/bash -g lfs -m -k /dev/null lfs

```

### OUTPUT
```
root:~# echo $LFS
/mnt/lfs

root:~# mkdir -pv $LFS/{bin,etc,lib,sbin,usr,var}
mkdir: created directory '/mnt/lfs/bin'
mkdir: created directory '/mnt/lfs/etc'
mkdir: created directory '/mnt/lfs/lib'
mkdir: created directory '/mnt/lfs/sbin'
mkdir: created directory '/mnt/lfs/usr'
mkdir: created directory '/mnt/lfs/var'

root:~# case $(uname -m) in
>   x86_64) mkdir -pv $LFS/lib64 ;;
> esac
mkdir: created directory '/mnt/lfs/lib64'
root:~# mkdir -pv $LFS/tools
mkdir: created directory '/mnt/lfs/tools'

root:~# groupadd lfs

root:~# useradd -s /bin/bash -g lfs -m -k /dev/null lfs

root:~# 

```

<br>
### INPUT
```
[ ! -e /etc/bash.bashrc ] || mv -v /etc/bash.bashrc /etc/bash.bashrc.NOUSE
passwd lfs
chown -v lfs $LFS/{usr,lib,var,etc,bin,sbin,tools}
case $(uname -m) in
  x86_64) chown -v lfs $LFS/lib64 ;;
esac
chown -v lfs $LFS/sources
su - lfs

```

### OUTPUT
```
root:~# [ ! -e /etc/bash.bashrc ] || mv -v /etc/bash.bashrc /etc/bash.bashrc.NOUSE
renamed '/etc/bash.bashrc' -> '/etc/bash.bashrc.NOUSE'

root:~# passwd lfs
New password: *******
Retype new password: *******
passwd: password updated successfully

root:~# chown -v lfs $LFS/{usr,lib,var,etc,bin,sbin,tools}
changed ownership of '/mnt/lfs/usr' from root to lfs
changed ownership of '/mnt/lfs/lib' from root to lfs
changed ownership of '/mnt/lfs/var' from root to lfs
changed ownership of '/mnt/lfs/etc' from root to lfs
changed ownership of '/mnt/lfs/bin' from root to lfs
changed ownership of '/mnt/lfs/sbin' from root to lfs
changed ownership of '/mnt/lfs/tools' from root to lfs

root:~# case $(uname -m) in
>   x86_64) chown -v lfs $LFS/lib64 ;;
> esac
changed ownership of '/mnt/lfs/lib64' from root to lfs

root:~# chown -v lfs $LFS/sources
changed ownership of '/mnt/lfs/sources' from root to lfs

root:~# su - lfs

lfs@osp:~$

```

<br>
### INPUT
```
cat > ~/.bash_profile << "EOF"
exec env -i HOME=$HOME TERM=$TERM PS1='\u:\w\$ ' /bin/bash
EOF
cat > ~/.bashrc << "EOF"
set +h
umask 022
LFS=/mnt/lfs
LC_ALL=POSIX
LFS_TGT=$(uname -m)-lfs-linux-gnu
PATH=/usr/bin
if [ ! -L /bin ]; then PATH=/bin:$PATH; fi
PATH=$LFS/tools/bin:$PATH
MAKEFLAGS='-j8'
export LFS LC_ALL LFS_TGT PATH MAKEFLAGS
EOF
source ~/.bash_profile

```

### OUTPUT
```
lfs@osp:~$ cat > ~/.bash_profile << "EOF"
> exec env -i HOME=$HOME TERM=$TERM PS1='\u:\w\$ ' /bin/bash
> EOF

lfs@osp:~$ cat > ~/.bashrc << "EOF"
> set +h
> umask 022
> LFS=/mnt/lfs
> LC_ALL=POSIX
> LFS_TGT=$(uname -m)-lfs-linux-gnu
> PATH=/usr/bin
> if [ ! -L /bin ]; then PATH=/bin:$PATH; fi
> PATH=$LFS/tools/bin:$PATH
> export LFS LC_ALL LFS_TGT PATH
> EOF

lfs:~$ source ~/.bash_profile

lfs:~$ 

```

<br>
### INPUT
```
exit

```


### OUTPUT
```
lfs:~$ exit
exit

root:~# 

```

<br>
### INPUT
```
shutdown -h now

```

### OUTPUT
```
root:~# shutdown -h now
Connection to localhost closed by remote host.
Connection to localhost closed.

rms46@pamulang1:~$

```

* Back to "pamulang1" host

* Create LFS-04.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-05.md)
[NEXT](LFS-07.md)
<br>

