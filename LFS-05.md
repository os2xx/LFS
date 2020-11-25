---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-04.md)
[NEXT](LFS-06.md)

# LFS: Chapter 3

<br>
<img src="pictures/LFS-A37.jpg" width="960">

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

root@osp:~#

```

<br>
### INPUT
```
echo $LFS
mkdir -v $LFS/sources
chmod -v a+wt $LFS/sources
exit

```

### OUTPUT
```
root@osp:~# echo $LFS
/mnt/lfs

root@osp:~# mkdir -v $LFS/sources
mkdir: created directory '/mnt/lfs/sources'

root@osp:~# chmod -v a+wt $LFS/sources
mode of '/mnt/lfs/sources' changed from 0755 (rwxr-xr-x) to 1777 (rwxrwxrwt)

root@osp:~# exit
logout

cbkadal@osp:~$ 

```

<br>
### INPUT
```
echo $LFS
wget --continue http://www.linuxfromscratch.org/lfs/view/stable/wget-list
wget --input-file=wget-list --continue --directory-prefix=$LFS/sources
pushd $LFS/sources
  wget --continue http://www.linuxfromscratch.org/lfs/view/stable/md5sums
  md5sum -c md5sums
popd

```

### OUTPUT
```
cbkadal@osp:~$ echo $LFS
/mnt/lfs

cbkadal@osp:~$ wget --continue http://www.linuxfromscratch.org/lfs/view/stable/wget-list
--2020-11-21 21:45:22--  http://www.linuxfromscratch.org/lfs/view/stable/wget-list
Resolving www.linuxfromscratch.org (www.linuxfromscratch.org)... 192.155.86.174, 2600:3c01::f03c:91ff:fe70:25e8
Connecting to www.linuxfromscratch.org (www.linuxfromscratch.org)|192.155.86.174|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5412 (5.3K)
Saving to: 'wget-list'

wget-list                     100%[=================================================>]   5.29K  --.-KB/s    in 0.002s  

2020-11-21 21:45:23 (2.73 MB/s) - 'wget-list' saved [5412/5412]


cbkadal@osp:~$ wget --input-file=wget-list --continue --directory-prefix=$LFS/sources
--2020-11-21 21:45:23--  http://download.savannah.gnu.org/releases/acl/acl-2.2.53.tar.gz
Resolving download.savannah.gnu.org (download.savannah.gnu.org)... 209.51.188.200, 2001:470:142:5::200
Connecting to download.savannah.gnu.org (download.savannah.gnu.org)|209.51.188.200|:80... connected.
HTTP request sent, awaiting response... 302 Moved Temporarily
Location: http://download-mirror.savannah.gnu.org/releases/acl/acl-2.2.53.tar.gz [following]
--2020-11-21 21:45:24--  http://download-mirror.savannah.gnu.org/releases/acl/acl-2.2.53.tar.gz
Resolving download-mirror.savannah.gnu.org (download-mirror.savannah.gnu.org)... 209.51.188.200
Reusing existing connection to download.savannah.gnu.org:80.
HTTP request sent, awaiting response... 200 OK
Length: 524300 (512K) [application/octet-stream]
Saving to: '/mnt/lfs/sources/acl-2.2.53.tar.gz'

acl-2.2.53.tar.gz             100%[=================================================>] 512.01K   415KB/s    in 1.2s    

2020-11-21 21:45:26 (415 KB/s) - '/mnt/lfs/sources/acl-2.2.53.tar.gz' saved [524300/524300]

===== TL;DR =====

--2020-11-21 21:55:24--  http://www.linuxfromscratch.org/patches/lfs/10.0/sysvinit-2.97-consolidated-1.patch
Reusing existing connection to www.linuxfromscratch.org:80.
HTTP request sent, awaiting response... 200 OK
Length: 2468 (2.4K) [text/x-diff]
Saving to: '/mnt/lfs/sources/sysvinit-2.97-consolidated-1.patch'

sysvinit-2.97-consolidated-1. 100%[=================================================>]   2.41K  --.-KB/s    in 0s      

2020-11-21 21:55:24 (119 MB/s) - '/mnt/lfs/sources/sysvinit-2.97-consolidated-1.patch' saved [2468/2468]

FINISHED --2020-11-21 21:55:24--
Total wall clock time: 10m 1s
Downloaded: 88 files, 423M in 7m 27s (968 KB/s)


cbkadal@osp:~$ pushd $LFS/sources
/mnt/lfs/sources ~
cbkadal@osp:/mnt/lfs/sources$   wget --continue http://www.linuxfromscratch.org/lfs/view/stable/md5sums
--2020-11-21 22:00:43--  http://www.linuxfromscratch.org/lfs/view/stable/md5sums
Resolving www.linuxfromscratch.org (www.linuxfromscratch.org)... 192.155.86.174, 2600:3c01::f03c:91ff:fe70:25e8
Connecting to www.linuxfromscratch.org (www.linuxfromscratch.org)|192.155.86.174|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4647 (4.5K)
Saving to: 'md5sums'

md5sums                       100%[=================================================>]   4.54K  --.-KB/s    in 0s      

2020-11-21 22:00:43 (168 MB/s) - 'md5sums' saved [4647/4647]

cbkadal@osp:/mnt/lfs/sources$   md5sum -c md5sums
acl-2.2.53.tar.gz: OK
attr-2.4.48.tar.gz: OK
autoconf-2.69.tar.xz: OK

===== TL;DR =====

glibc-2.32-fhs-1.patch: OK
kbd-2.3.0-backspace-1.patch: OK
sysvinit-2.97-consolidated-1.patch: OK
cbkadal@osp:/mnt/lfs/sources$ popd
~
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

root@osp:~#

```

<br>
### INPUT
```
shutdown -h now

```

### OUTPUT
```
root@osp:~# shutdown -h now
Connection to localhost closed by remote host.
Connection to localhost closed.

rms46@pamulang1:~$

```

* Back to "pamulang1" host

* Create LFS-03.OVA (backup)


<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-04.md)
[NEXT](LFS-06.md)
<br>

