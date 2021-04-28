---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-02.md)
[NEXT](LFS-04.md)

<br>
# LFS: Chapter 3

## Virtual Box Guest LFS-03

* Import LFS-02.ova, rename to LFS-03

<br>
<img src="pictures/LFS-A37.jpg" width="960">

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
ls -al $LFS/
mkdir -v $LFS/sources
chmod -v a+wt $LFS/sources
ls -al $LFS/
exit

```

### OUTPUT-03
```
root:~# ls -al $LFS/
total 24
drwxr-xr-x 3 root root  4096 Apr 27 22:50 .
drwxr-xr-x 3 root root  4096 Apr 27 22:50 ..
drwx------ 2 root root 16384 Apr 28 05:42 lost+found

root:~# mkdir -v $LFS/sources
mkdir: created directory '/mnt/lfs/sources'

root:~# chmod -v a+wt $LFS/sources
mode of '/mnt/lfs/sources' changed from 0755 (rwxr-xr-x) to 1777 (rwxrwxrwt)

root:~# ls -al $LFS/
total 28
drwxr-xr-x 4 root root  4096 Apr 28 06:09 .
drwxr-xr-x 3 root root  4096 Apr 27 22:50 ..
drwx------ 2 root root 16384 Apr 28 05:42 lost+found
drwxrwxrwt 2 root root  4096 Apr 28 06:09 sources

root:~# exit
logout

cbkadal:~$ 

```

<br>
### INPUT-04
```
ls -al $LFS/
wget --continue http://www.linuxfromscratch.org/lfs/view/stable/wget-list
wget --input-file=wget-list --continue --directory-prefix=$LFS/sources
pushd $LFS/sources
  wget --continue http://www.linuxfromscratch.org/lfs/view/stable/md5sums
  md5sum -c md5sums
popd

```

### OUTPUT-04
```
cbkadal:~$ ls -al $LFS/
total 28
drwxr-xr-x 4 root root  4096 Apr 28 06:09 .
drwxr-xr-x 3 root root  4096 Apr 27 22:50 ..
drwx------ 2 root root 16384 Apr 28 05:42 lost+found
drwxrwxrwt 2 root root  4096 Apr 28 06:09 sources

cbkadal:~$ wget --continue http://www.linuxfromscratch.org/lfs/view/stable/wget-list
===== TL;DR =====
2021-04-28 06:29:34 (3.36 MB/s) - ‘wget-list’ saved [5423/5423]

cbkadal:~$ wget --input-file=wget-list --continue --directory-prefix=$LFS/sources
===== TL;DR =====
2021-04-28 06:29:35 (1.66 MB/s) - ‘/mnt/lfs/sources/acl-2.2.53.tar.gz’ saved [524300/524300]
2021-04-28 06:29:37 (967 KB/s)  - ‘/mnt/lfs/sources/attr-2.4.48.tar.gz’ saved [467840/467840]
2021-04-28 06:29:41 (538 KB/s)  - ‘/mnt/lfs/sources/autoconf-2.71.tar.xz’ saved [1292296/1292296]
2021-04-28 06:29:44 (590 KB/s)  - ‘/mnt/lfs/sources/automake-1.16.3.tar.xz’ saved [1590708/1590708]
2021-04-28 06:29:58 (742 KB/s)  - ‘/mnt/lfs/sources/bash-5.1.tar.gz’ saved [10458638/10458638]
2021-04-28 06:29:59 (1.39 MB/s) - ‘/mnt/lfs/sources/bc-3.3.0.tar.xz’ saved [229580/229580]
2021-04-28 06:30:22 (1.02 MB/s) - ‘/mnt/lfs/sources/binutils-2.36.1.tar.xz’ saved [22772248/22772248]
2021-04-28 06:30:25 (726 KB/s)  - ‘/mnt/lfs/sources/bison-3.7.5.tar.xz’ saved [2622228/2622228]
2021-04-28 06:30:29 (363 KB/s)  - ‘/mnt/lfs/sources/bzip2-1.0.8.tar.gz’ saved [810029/810029]
2021-04-28 06:30:31 (1.64 MB/s) - ‘/mnt/lfs/sources/check-0.15.2.tar.gz’ saved [774985/774985]
2021-04-28 06:30:38 (824 KB/s)  - ‘/mnt/lfs/sources/coreutils-8.32.tar.xz’ saved [5547836/5547836]
2021-04-28 06:30:41 (972 KB/s)  - ‘/mnt/lfs/sources/dbus-1.12.20.tar.gz’ saved [2095511/2095511]
2021-04-28 06:30:44 (332 KB/s)  - ‘/mnt/lfs/sources/dejagnu-1.6.2.tar.gz’ saved [525879/525879]
2021-04-28 06:30:47 (494 KB/s)  - ‘/mnt/lfs/sources/diffutils-3.7.tar.xz’ saved [1448828/1448828]
2021-04-28 06:30:54 (1.56 MB/s) - ‘/mnt/lfs/sources/e2fsprogs-1.46.1.tar.gz’ saved [9490551/9490551]
2021-04-28 06:31:05 (856 KB/s)  - ‘/mnt/lfs/sources/elfutils-0.183.tar.bz2’ saved [9109254/9109254]
2021-04-28 06:31:23 (120 KB/s)  - ‘/mnt/lfs/sources/eudev-3.2.10.tar.gz’ saved [1961960/1961960]
2021-04-28 06:31:26 (1.17 MB/s) - ‘/mnt/lfs/sources/expat-2.2.10.tar.xz’ saved [425432/425432]
2021-04-28 06:31:30 (1.24 MB/s) - ‘/mnt/lfs/sources/expect5.45.4.tar.gz’ saved [632363/632363]
2021-04-28 06:31:35 (243 KB/s)  - ‘/mnt/lfs/sources/file-5.39.tar.gz’ saved [954266/954266]
2021-04-28 06:31:39 (649 KB/s)  - ‘/mnt/lfs/sources/findutils-4.8.0.tar.xz’ saved [1983096/1983096]
2021-04-28 06:31:42 (1.70 MB/s) - ‘/mnt/lfs/sources/flex-2.6.4.tar.gz’ saved [1419096/1419096]
2021-04-28 06:31:47 (729 KB/s)  - ‘/mnt/lfs/sources/gawk-5.1.0.tar.xz’ saved [3154564/3154564]
2021-04-28 06:32:46 (1.21 MB/s) - ‘/mnt/lfs/sources/gcc-10.2.0.tar.xz’ saved [75004144/75004144]
2021-04-28 06:32:48 (656 KB/s)  - ‘/mnt/lfs/sources/gdbm-1.19.tar.gz’ saved [967861/967861]
2021-04-28 06:33:00 (813 KB/s)  - ‘/mnt/lfs/sources/gettext-0.21.tar.xz’ saved [9714352/9714352]
2021-04-28 06:33:16 (1.04 MB/s) - ‘/mnt/lfs/sources/glibc-2.33.tar.xz’ saved [17031280/17031280]
2021-04-28 06:33:19 (691 KB/s)  - ‘/mnt/lfs/sources/gmp-6.2.1.tar.xz’ saved [2027316/2027316]
2021-04-28 06:33:21 (577 KB/s)  - ‘/mnt/lfs/sources/gperf-3.1.tar.gz’ saved [1215925/1215925]
2021-04-28 06:33:25 (544 KB/s)  - ‘/mnt/lfs/sources/grep-3.6.tar.xz’ saved [1589412/1589412]
2021-04-28 06:33:32 (571 KB/s)  - ‘/mnt/lfs/sources/groff-1.22.4.tar.gz’ saved [4137480/4137480]
2021-04-28 06:33:42 (627 KB/s)  - ‘/mnt/lfs/sources/grub-2.04.tar.xz’ saved [6393864/6393864]
2021-04-28 06:33:44 (554 KB/s)  - ‘/mnt/lfs/sources/gzip-1.10.tar.xz’ saved [775144/775144]
2021-04-28 06:33:45 (1.61 MB/s) - ‘/mnt/lfs/sources/iana-etc-20210202.tar.gz’ saved [591414/591414]
2021-04-28 06:33:49 (554 KB/s)  - ‘/mnt/lfs/sources/inetutils-2.0.tar.xz’ saved [1496632/1496632]
2021-04-28 06:33:52 (271 KB/s)  - ‘/mnt/lfs/sources/intltool-0.51.0.tar.gz’ saved [162286/162286]
2021-04-28 06:33:54 (584 KB/s)  - ‘/mnt/lfs/sources/iproute2-5.10.0.tar.xz’ saved [798776/798776]
2021-04-28 06:33:57 (584 KB/s)  - ‘/mnt/lfs/sources/kbd-2.4.0.tar.xz’ saved [1120700/1120700]
2021-04-28 06:33:58 (576 KB/s)  - ‘/mnt/lfs/sources/kmod-28.tar.xz’ saved [552448/552448]
2021-04-28 06:34:00 (339 KB/s)  - ‘/mnt/lfs/sources/less-563.tar.gz’ saved [335508/335508]
2021-04-28 06:34:01 (135 KB/s)  - ‘/mnt/lfs/sources/lfs-bootscripts-20210201.tar.xz’ saved [32596/32596]
2021-04-28 06:34:02 (553 KB/s)  - ‘/mnt/lfs/sources/libcap-2.48.tar.xz’ saved [132280/132280]
2021-04-28 06:34:06 (455 KB/s)  - ‘/mnt/lfs/sources/libffi-3.3.tar.gz’ saved [1305466/1305466]
2021-04-28 06:34:08 (1.30 MB/s) - ‘/mnt/lfs/sources/libpipeline-1.5.3.tar.gz’ saved [994663/994663]
2021-04-28 06:34:11 (438 KB/s)  - ‘/mnt/lfs/sources/libtool-2.4.6.tar.xz’ saved [973080/973080]
2021-04-28 06:36:14 (928 KB/s)  - ‘/mnt/lfs/sources/linux-5.10.17.tar.xz’ saved [116272648/116272648]
2021-04-28 06:36:18 (487 KB/s)  - ‘/mnt/lfs/sources/m4-1.4.18.tar.xz’ saved [1207688/1207688]
2021-04-28 06:36:22 (593 KB/s)  - ‘/mnt/lfs/sources/make-4.3.tar.gz’ saved [2317073/2317073]
2021-04-28 06:36:24 (1.49 MB/s) - ‘/mnt/lfs/sources/man-db-2.9.4.tar.xz’ saved [1909020/1909020]
2021-04-28 06:36:26 (1.07 MB/s) - ‘/mnt/lfs/sources/man-pages-5.10.tar.xz’ saved [1747688/1747688]
2021-04-28 06:36:29 (1.75 MB/s) - ‘/mnt/lfs/sources/meson-0.57.1.tar.gz’ saved [1849222/1849222]
2021-04-28 06:36:32 (432 KB/s)  - ‘/mnt/lfs/sources/mpc-1.2.1.tar.gz’ saved [838731/838731]
2021-04-28 06:36:36 (553 KB/s)  - ‘/mnt/lfs/sources/mpfr-4.1.0.tar.xz’ saved [1525476/1525476]
2021-04-28 06:36:42 (710 KB/s)  - ‘/mnt/lfs/sources/ncurses-6.2.tar.gz’ saved [3425862/3425862]
2021-04-28 06:36:45 (248 KB/s)  - ‘/mnt/lfs/sources/ninja-1.10.2.tar.gz’ saved [213959/213959]
2021-04-28 06:36:51 (1.74 MB/s) - ‘/mnt/lfs/sources/openssl-1.1.1j.tar.gz’ saved [9823161/9823161]
2021-04-28 06:36:54 (420 KB/s)  - ‘/mnt/lfs/sources/patch-2.7.6.tar.xz’ saved [783756/783756]
2021-04-28 06:37:01 (1.72 MB/s) - ‘/mnt/lfs/sources/perl-5.32.1.tar.xz’ saved [12610988/12610988]
2021-04-28 06:37:04 (925 KB/s)  - ‘/mnt/lfs/sources/pkg-config-0.29.2.tar.gz’ saved [2016830/2016830]
2021-04-28 06:37:07 (1.34 MB/s) - ‘/mnt/lfs/sources/procps-ng-3.3.17.tar.xz’ saved [1008428/1008428]
2021-04-28 06:37:10 (1.17 MB/s) - ‘/mnt/lfs/sources/psmisc-23.4.tar.xz’ saved [370000/370000]
2021-04-28 06:37:21 (1.72 MB/s) - ‘/mnt/lfs/sources/Python-3.9.2.tar.xz’ saved [18889164/18889164]
2021-04-28 06:37:25 (1.72 MB/s) - ‘/mnt/lfs/sources/python-3.9.2-docs-html.tar.bz2’ saved [6818470/6818470]
2021-04-28 06:37:30 (661 KB/s)  - ‘/mnt/lfs/sources/readline-8.1.tar.gz’ saved [2993288/2993288]
2021-04-28 06:37:33 (564 KB/s)  - ‘/mnt/lfs/sources/sed-4.8.tar.xz’ saved [1348048/1348048]
2021-04-28 06:37:35 (1.31 MB/s) - ‘/mnt/lfs/sources/shadow-4.8.1.tar.xz’ saved [1611196/1611196]
2021-04-28 06:37:37 (224 KB/s)  - ‘/mnt/lfs/sources/sysklogd-1.5.1.tar.gz’ saved [90011/90011]
2021-04-28 06:37:46 (1.29 MB/s) - ‘/mnt/lfs/sources/systemd-247.tar.gz’ saved [9887080]
2021-04-28 06:37:48 (398 KB/s)  - ‘/mnt/lfs/sources/systemd-man-pages-247.tar.xz’ saved [623736/623736]
2021-04-28 06:37:49 (604 KB/s)  - ‘/mnt/lfs/sources/sysvinit-2.98.tar.xz’ saved [127028/127028]
2021-04-28 06:37:54 (630 KB/s)  - ‘/mnt/lfs/sources/tar-1.34.tar.xz’ saved [2226068/2226068]
2021-04-28 06:38:03 (1.64 MB/s) - ‘/mnt/lfs/sources/tcl8.6.11-src.tar.gz’ saved [10259009/10259009]
2021-04-28 06:38:06 (1.48 MB/s) - ‘/mnt/lfs/sources/tcl8.6.11-html.tar.gz’ saved [1199155/1199155]
2021-04-28 06:38:13 (712 KB/s)  - ‘/mnt/lfs/sources/texinfo-6.7.tar.xz’ saved [4337984/4337984]
2021-04-28 06:38:16 (621 KB/s)  - ‘/mnt/lfs/sources/tzdata2021a.tar.gz’ saved [411892/411892]
2021-04-28 06:38:16 (3.29 MB/s) - ‘/mnt/lfs/sources/udev-lfs-20171102.tar.xz’ saved [10280/10280]
2021-04-28 06:38:21 (1.15 MB/s) - ‘/mnt/lfs/sources/util-linux-2.36.2.tar.xz’ saved [5348032/5348032]
2021-04-28 06:38:39 (890 KB/s)  - ‘/mnt/lfs/sources/vim-8.2.2433.tar.gz’ saved [15411342/15411342]
2021-04-28 06:38:39 (1.12 MB/s) - ‘/mnt/lfs/sources/XML-Parser-2.46.tar.gz’ saved [254763/254763]
2021-04-28 06:38:43 (1.26 MB/s) - ‘/mnt/lfs/sources/xz-5.2.5.tar.xz’ saved [1148824/1148824]
2021-04-28 06:38:47 (180 KB/s)  - ‘/mnt/lfs/sources/zlib-1.2.11.tar.xz’ saved [467960/467960]
2021-04-28 06:38:49 (1.75 MB/s) - ‘/mnt/lfs/sources/zstd-1.4.8.tar.gz’ saved [1803550/1803550]
2021-04-28 06:38:50 (7.79 MB/s) - ‘/mnt/lfs/sources/bzip2-1.0.8-install_docs-1.patch’ saved [1684/1684]
2021-04-28 06:38:51 (215 KB/s)  - ‘/mnt/lfs/sources/coreutils-8.32-i18n-1.patch’ saved [169325/169325]
2021-04-28 06:38:51 (53.0 MB/s) - ‘/mnt/lfs/sources/glibc-2.33-fhs-1.patch’ saved [2804/2804]
2021-04-28 06:38:52 (2.37 MB/s) - ‘/mnt/lfs/sources/kbd-2.4.0-backspace-1.patch’ saved [12640/12640]
2021-04-28 06:38:52 (45.3 MB/s) - ‘/mnt/lfs/sources/sysvinit-2.98-consolidated-1.patch’ saved [2468/2468]
2021-04-28 06:38:52 (27.7 MB/s) - ‘/mnt/lfs/sources/systemd-247-upstream_fixes-1.patch’ saved [5935/5935]
2021-04-28 06:38:53 (4.18 MB/s) - ‘md5sums’ saved [4586/4586]

===== TL;DR =====
$   md5sum -c md5sums
acl-2.2.53.tar.gz: OK
attr-2.4.48.tar.gz: OK
autoconf-2.71.tar.xz: OK
automake-1.16.3.tar.xz: OK
bash-5.1.tar.gz: OK
bc-3.3.0.tar.xz: OK
binutils-2.36.1.tar.xz: OK
bison-3.7.5.tar.xz: OK
bzip2-1.0.8.tar.gz: OK
check-0.15.2.tar.gz: OK
coreutils-8.32.tar.xz: OK
dejagnu-1.6.2.tar.gz: OK
diffutils-3.7.tar.xz: OK
e2fsprogs-1.46.1.tar.gz: OK
elfutils-0.183.tar.bz2: OK
eudev-3.2.10.tar.gz: OK
expat-2.2.10.tar.xz: OK
expect5.45.4.tar.gz: OK
file-5.39.tar.gz: OK
findutils-4.8.0.tar.xz: OK
flex-2.6.4.tar.gz: OK
gawk-5.1.0.tar.xz: OK
gcc-10.2.0.tar.xz: OK
gdbm-1.19.tar.gz: OK
gettext-0.21.tar.xz: OK
glibc-2.33.tar.xz: OK
gmp-6.2.1.tar.xz: OK
gperf-3.1.tar.gz: OK
grep-3.6.tar.xz: OK
groff-1.22.4.tar.gz: OK
grub-2.04.tar.xz: OK
gzip-1.10.tar.xz: OK
iana-etc-20210202.tar.gz: OK
inetutils-2.0.tar.xz: OK
intltool-0.51.0.tar.gz: OK
iproute2-5.10.0.tar.xz: OK
kbd-2.4.0.tar.xz: OK
kmod-28.tar.xz: OK
less-563.tar.gz: OK
lfs-bootscripts-20210201.tar.xz: OK
libcap-2.48.tar.xz: OK
libffi-3.3.tar.gz: OK
libpipeline-1.5.3.tar.gz: OK
libtool-2.4.6.tar.xz: OK
linux-5.10.17.tar.xz: OK
m4-1.4.18.tar.xz: OK
make-4.3.tar.gz: OK
man-db-2.9.4.tar.xz: OK
man-pages-5.10.tar.xz: OK
meson-0.57.1.tar.gz: OK
mpc-1.2.1.tar.gz: OK
mpfr-4.1.0.tar.xz: OK
ncurses-6.2.tar.gz: OK
ninja-1.10.2.tar.gz: OK
openssl-1.1.1j.tar.gz: OK
patch-2.7.6.tar.xz: OK
perl-5.32.1.tar.xz: OK
pkg-config-0.29.2.tar.gz: OK
procps-ng-3.3.17.tar.xz: OK
psmisc-23.4.tar.xz: OK
Python-3.9.2.tar.xz: OK
python-3.9.2-docs-html.tar.bz2: OK
readline-8.1.tar.gz: OK
sed-4.8.tar.xz: OK
shadow-4.8.1.tar.xz: OK
sysklogd-1.5.1.tar.gz: OK
sysvinit-2.98.tar.xz: OK
tar-1.34.tar.xz: OK
tcl8.6.11-src.tar.gz: OK
tcl8.6.11-html.tar.gz: OK
texinfo-6.7.tar.xz: OK
tzdata2021a.tar.gz: OK
udev-lfs-20171102.tar.xz: OK
util-linux-2.36.2.tar.xz: OK
vim-8.2.2433.tar.gz: OK
XML-Parser-2.46.tar.gz: OK
xz-5.2.5.tar.xz: OK
zlib-1.2.11.tar.xz: OK
zstd-1.4.8.tar.gz: OK
bzip2-1.0.8-install_docs-1.patch: OK
coreutils-8.32-i18n-1.patch: OK
glibc-2.33-fhs-1.patch: OK
kbd-2.4.0-backspace-1.patch: OK
sysvinit-2.98-consolidated-1.patch: OK

```

<br>
### INPUT-05
```
su -

```

### OUTPUT-05
```
cbkadal@osp:~$ su -
Password: 

```

<br>
### INPUT-06
```
poweroff

```

### OUTPUT-06
```
root:~# poweroff 
Connection to localhost closed by remote host.
Connection to localhost closed.

rms46@pamulang1:~$ 

```

* Back to "pamulang1" host

* Export LFS-03.OVA (backup)


<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-02.md)
[NEXT](LFS-04.md)
<br>

