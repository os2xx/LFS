---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-06.md)
[NEXT](index.md)

<br>
<img src="pictures/LFS-A38.jpg" width="960">

```
ssh -p 6024 cbkadal@localhost

```

```
rms46@pamulang1:~$ ssh -p 6024 cbkadal@localhost
cbkadal@localhost's password:

===== TL;DR =====

cbkadal@osp:~$ 

```

```
su -
```

```
cbkadal@osp:~$ su -
Password:

root@osp:~#

```

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


```
root@osp:~# echo $LFS
/mnt/lfs

root@osp:~# mkdir -pv $LFS/{bin,etc,lib,sbin,usr,var}
mkdir: created directory '/mnt/lfs/bin'
mkdir: created directory '/mnt/lfs/etc'
mkdir: created directory '/mnt/lfs/lib'
mkdir: created directory '/mnt/lfs/sbin'
mkdir: created directory '/mnt/lfs/usr'
mkdir: created directory '/mnt/lfs/var'

root@osp:~# case $(uname -m) in
>   x86_64) mkdir -pv $LFS/lib64 ;;
> esac
mkdir: created directory '/mnt/lfs/lib64'
root@osp:~# mkdir -pv $LFS/tools
mkdir: created directory '/mnt/lfs/tools'

root@osp:~# groupadd lfs

root@osp:~# useradd -s /bin/bash -g lfs -m -k /dev/null lfs

root@osp:~# 

```

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

```
root@osp:~# [ ! -e /etc/bash.bashrc ] || mv -v /etc/bash.bashrc /etc/bash.bashrc.NOUSE
renamed '/etc/bash.bashrc' -> '/etc/bash.bashrc.NOUSE'

root@osp:~# passwd lfs
New password: *******
Retype new password: *******
passwd: password updated successfully

root@osp:~# chown -v lfs $LFS/{usr,lib,var,etc,bin,sbin,tools}
changed ownership of '/mnt/lfs/usr' from root to lfs
changed ownership of '/mnt/lfs/lib' from root to lfs
changed ownership of '/mnt/lfs/var' from root to lfs
changed ownership of '/mnt/lfs/etc' from root to lfs
changed ownership of '/mnt/lfs/bin' from root to lfs
changed ownership of '/mnt/lfs/sbin' from root to lfs
changed ownership of '/mnt/lfs/tools' from root to lfs

root@osp:~# case $(uname -m) in
>   x86_64) chown -v lfs $LFS/lib64 ;;
> esac
changed ownership of '/mnt/lfs/lib64' from root to lfs

root@osp:~# chown -v lfs $LFS/sources
changed ownership of '/mnt/lfs/sources' from root to lfs

root@osp:~# su - lfs

lfs@osp:~$

```

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

```
exit

```


```
lfs:~$ exit
exit

root@osp:~# 
```

```
shutdown -h now

```

```
root@osp:~# shutdown -h now
Connection to localhost closed by remote host.
Connection to localhost closed.

rms46@pamulang1:~$

```

* Back to "pamulang1" host

* Create LFS-05.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-06.md)
[NEXT](index.md)
<br>

