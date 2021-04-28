---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-03.md)
[NEXT](LFS-07.md)

<br>
# LFS: Chapter 4

## Virtual Box Guest LFS-04

* Import LFS-03.ova, rename to LFS-04

<br>
<img src="pictures/LFS-A38.jpg" width="960">

<br>
### INPUT
```
ssh -p 6023 cbkadal@localhost

```

### OUTPUT
```
rms46@pamulang1:~$ ssh -p 6023 cbkadal@localhost
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
[ ! -e /etc/bash.bashrc ] || mv -v /etc/bash.bashrc /etc/bash.bashrc.NOUSE
passwd lfs

```

### OUTPUT
```

```

<br>
### INPUT
```
chown -v lfs $LFS/{usr,lib,var,etc,bin,sbin,tools}
case $(uname -m) in
  x86_64) chown -v lfs $LFS/lib64 ;;
esac
chown -v lfs $LFS/sources
su - lfs

```

### OUTPUT
```


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
poweroff

```

### OUTPUT
```

```

* Back to "pamulang1" host

* Export LFS-04.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-03.md)
[NEXT](LFS-07.md)
<br>

