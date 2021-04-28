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
# LFS: Chapter 4

## Virtual Box Guest LFS-04

* Import LFS-03.ova, rename to LFS-04

<br>
<img src="pictures/LFS-A38.jpg" width="960">

<br>
### INPUT-01
```
ssh -p 6023 cbkadal@localhost

```

### OUTPUT-01
```
rms46@pamulang1:~$ ssh -p 6023 cbkadal@localhost
cbkadal@localhost's password:

===== TL;DR =====

cbkadal@osp:~$ 

```

<br>
### INPUT-02
```
su -

```

### OUTPUT-02
```
cbkadal@osp:~$ su -
Password:

root:~#

```

<br>
### INPUT-03
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

### OUTPUT-03
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

root:~# [ ! -e /etc/bash.bashrc ] || mv -v /etc/bash.bashrc /etc/bash.bashrc.NOUSE
renamed '/etc/bash.bashrc' -> '/etc/bash.bashrc.NOUSE'

root:~# passwd lfs
New password: 
Retype new password: 
passwd: password updated successfully

root:~# 

```

<br>
### INPUT-04
```
chown -v lfs $LFS/{usr,lib,var,etc,bin,sbin,tools}
case $(uname -m) in
  x86_64) chown -v lfs $LFS/lib64 ;;
esac
chown -v lfs $LFS/sources
su - lfs

```

### OUTPUT-04
```
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

-bash-5.0$

```

<br>
### INPUT-05
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

### OUTPUT-05
```
-bash-5.0$ cat > ~/.bash_profile << "EOF"
> exec env -i HOME=$HOME TERM=$TERM PS1='\u:\w\$ ' /bin/bash
> EOF
-bash-5.0$ cat > ~/.bashrc << "EOF"
> set +h
> umask 022
> LFS=/mnt/lfs
> LC_ALL=POSIX
> LFS_TGT=$(uname -m)-lfs-linux-gnu
> PATH=/usr/bin
> if [ ! -L /bin ]; then PATH=/bin:$PATH; fi
> PATH=$LFS/tools/bin:$PATH
> MAKEFLAGS='-j8'
> export LFS LC_ALL LFS_TGT PATH MAKEFLAGS
> EOF
-bash-5.0$ source ~/.bash_profile

lfs:~$

```

<br>
### INPUT-06
```
exit

```


### OUTPUT-06
```
lfs:~$ exit
exit

root:~# 

```

<br>
### INPUT-07
```
poweroff

```

### OUTPUT-07
```
root:~# poweroff
Connection to localhost closed by remote host.
Connection to localhost closed.

rms46@pamulang1:~$

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
[NEXT](LFS-05.md)
<br>

