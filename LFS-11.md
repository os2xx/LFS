---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-10.md)
[NEXT](index.md)

<br>
<span style="color:red; font-weight:bold; font-size:larger;">
It is assumed that you understand how install a Debian VirtualBox Guest.
If you have never installed a VirtualBox Guest before, visit [OSP4DISS](https://osp4diss.vlsm.org/).
</span>

<br>
# LFS: Chapter 8

## Virtual Box Guest LFS-08

* Import LFS-07.ova, rename to LFS-08

<br>
<img src="pictures/LFS-A44.jpg" width="960">

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

<br>
# Mounting AGAIN (/dev and the File System)

### INPUT
```
echo $LFS
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
root:~# echo $LFS
/mnt/lfs

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

### INPUT
```
df
chroot "$LFS" /usr/bin/env -i   \
    HOME=/root                  \
    TERM="$TERM"                \
    PS1='(lfs chroot) \u:\w\$ ' \
    PATH=/bin:/usr/bin:/sbin:/usr/sbin \
    MAKEFLAGS='-j6' \
    /bin/bash --login +h
df

```

### OUTPUT
```
root:~# df
Filesystem     1K-blocks    Used Available Use% Mounted on
udev             4020860       0   4020860   0% /dev
tmpfs             807140    8644    798496   2% /run
/dev/sda1       16446332 2724804  12866388  18% /
tmpfs            4035684       0   4035684   0% /dev/shm
tmpfs               5120       0      5120   0% /run/lock
tmpfs            4035684       0   4035684   0% /sys/fs/cgroup
/dev/sdb1       32894736 2688836  28511900   9% /mnt/lfs
tmpfs             807136       0    807136   0% /run/user/1000
tmpfs            4035684       0   4035684   0% /mnt/lfs/run

root:~# chroot "$LFS" /usr/bin/env -i   \
>     HOME=/root                  \
>     TERM="$TERM"                \
>     PS1='(lfs chroot) \u:\w\$ ' \
>     PATH=/bin:/usr/bin:/sbin:/usr/sbin \
>     MAKEFLAGS='-j6' \
>     /bin/bash --login +h

(lfs chroot) root:/# df
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/sdb1       32894736 2688836  28511900   9% /
udev             4020860       0   4020860   0% /dev
tmpfs            4035684       0   4035684   0% /run

(lfs chroot) root:/# 

```

<br>
# Man-pages-5.08

### INPUT
```
cd /sources/
tar xf man-pages-5.08.tar.xz
cd man-pages-5.08/
time make install
cd ../
rm -rf man-pages-5.08/

```

### OUTPUT
```
(lfs chroot) root:~# cd /sources/

(lfs chroot) root:/sources# tar xf man-pages-5.08.tar.xz

(lfs chroot) root:/sources# cd man-pages-5.08/

(lfs chroot) root:/sources/man-pages-5.08# time make install
for i in man?; do \
	install -d -m 755 /usr/share/man/"$i" || exit $?; \
	install -m 644 "$i"/* /usr/share/man/"$i" || exit $?; \
done

real	0m0.090s
user	0m0.015s
sys	0m0.075s

(lfs chroot) root:/sources/man-pages-5.08# cd ../

(lfs chroot) root:/sources# rm -rf man-pages-5.08/

(lfs chroot) root:/sources# 

```

<br>
# Tcl-8.6.10

### INPUT
```
tar xf tcl8.6.10-src.tar.gz
cd tcl8.6.10/
tar -xf ../tcl8.6.10-html.tar.gz --strip-components=1

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf tcl8.6.10-src.tar.gz

(lfs chroot) root:/sources# cd tcl8.6.10/

(lfs chroot) root:/sources/tcl8.6.10# tar -xf ../tcl8.6.10-html.tar.gz --strip-components=1

(lfs chroot) root:/sources/tcl8.6.10# 

```

<br>
### INPUT
```
SRCDIR=$(pwd)
cd unix/
./configure --prefix=/usr           \
            --mandir=/usr/share/man \
            $([ "$(uname -m)" = x86_64 ] && echo --enable-64bit)

```

### OUTPUT
```
(lfs chroot) root:/sources/tcl8.6.10# SRCDIR=$(pwd)

(lfs chroot) root:/sources/tcl8.6.10# cd unix/

(lfs chroot) root:/sources/tcl8.6.10/unix# ./configure --prefix=/usr           \
>             --mandir=/usr/share/man \
>             $([ "$(uname -m)" = x86_64 ] && echo --enable-64bit)
checking whether to use symlinks for manpages... no
checking whether to compress the manpages... no

===== TL;DR =====

config.status: creating dltest/Makefile
config.status: creating tclConfig.sh
config.status: creating tcl.pc

(lfs chroot) root:/sources/tcl8.6.10/unix# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
===== TL;DR =====

s.o threadCmd.o threadSvCmd.o threadSpCmd.o threadPoolCmd.o psGdbm.o psLmdb.o threadSvListCmd.o threadSvKeylistCmd.o tclXkeylist.o threadUnix.o  -L/sources/tcl8.6.10/unix -ltclstub8.6 
: libthread2.8.5.so
make[1]: Leaving directory '/sources/tcl8.6.10/unix/pkgs/thread2.8.5'

real	0m58.700s
user	1m36.391s
sys	0m7.823s

(lfs chroot) root:/sources/tcl8.6.10/unix#

```

<br>
### INPUT
```
sed -e "s|$SRCDIR/unix|/usr/lib|" \
    -e "s|$SRCDIR|/usr/include|"  \
    -i tclConfig.sh

sed -e "s|$SRCDIR/unix/pkgs/tdbc1.1.1|/usr/lib/tdbc1.1.1|" \
    -e "s|$SRCDIR/pkgs/tdbc1.1.1/generic|/usr/include|"    \
    -e "s|$SRCDIR/pkgs/tdbc1.1.1/library|/usr/lib/tcl8.6|" \
    -e "s|$SRCDIR/pkgs/tdbc1.1.1|/usr/include|"            \
    -i pkgs/tdbc1.1.1/tdbcConfig.sh

sed -e "s|$SRCDIR/unix/pkgs/itcl4.2.0|/usr/lib/itcl4.2.0|" \
    -e "s|$SRCDIR/pkgs/itcl4.2.0/generic|/usr/include|"    \
    -e "s|$SRCDIR/pkgs/itcl4.2.0|/usr/include|"            \
    -i pkgs/itcl4.2.0/itclConfig.sh

unset SRCDIR

```

### OUTPUT
```
(lfs chroot) root:/sources/tcl8.6.10/unix# sed -e "s|$SRCDIR/unix|/usr/lib|" \
>     -e "s|$SRCDIR|/usr/include|"  \
>     -i tclConfig.sh

(lfs chroot) root:/sources/tcl8.6.10/unix# sed -e "s|$SRCDIR/unix/pkgs/tdbc1.1.1|/usr/lib/tdbc1.1.1|" \
>     -e "s|$SRCDIR/pkgs/tdbc1.1.1/generic|/usr/include|"    \
>     -e "s|$SRCDIR/pkgs/tdbc1.1.1/library|/usr/lib/tcl8.6|" \
>     -e "s|$SRCDIR/pkgs/tdbc1.1.1|/usr/include|"            \
>     -i pkgs/tdbc1.1.1/tdbcConfig.sh

(lfs chroot) root:/sources/tcl8.6.10/unix# sed -e "s|$SRCDIR/unix/pkgs/itcl4.2.0|/usr/lib/itcl4.2.0|" \
>     -e "s|$SRCDIR/pkgs/itcl4.2.0/generic|/usr/include|"    \
>     -e "s|$SRCDIR/pkgs/itcl4.2.0|/usr/include|"            \
>     -i pkgs/itcl4.2.0/itclConfig.sh

(lfs chroot) root:/sources/tcl8.6.10/unix# unset SRCDIR

(lfs chroot) root:/sources/tcl8.6.10/unix# 

```

<br>
### INPUT
```
time make test

```

### OUTPUT
```
===== TL;DR =====

Test files exiting with errors:  

  clock.test

real	4m15.176s
user	0m43.204s
sys	0m7.417s

(lfs chroot) root:/sources/tcl8.6.10/unix# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
===== TL;DR =====

Installing /sources/tcl8.6.10/pkgs/thread2.8.5/doc/man/ttrace.n
make[1]: Leaving directory '/sources/tcl8.6.10/unix/pkgs/thread2.8.5'
Installing and cross-linking command (.n) docs to /usr/share/man/mann/

real	0m4.219s
user	0m6.429s
sys	0m1.834s

(lfs chroot) root:/sources/tcl8.6.10/unix# 

```

<br>
### INPUT
```
chmod -v u+w /usr/lib/libtcl8.6.so
make install-private-headers
ln -sfv tclsh8.6 /usr/bin/tclsh
cd ../../
rm -rf tcl8.6.10/

```

### OUTPUT
```
(lfs chroot) root:/sources/tcl8.6.10/unix# chmod -v u+w /usr/lib/libtcl8.6.so
mode of '/usr/lib/libtcl8.6.so' changed from 0555 (r-xr-xr-x) to 0755 (rwxr-xr-x)

(lfs chroot) root:/sources/tcl8.6.10/unix# make install-private-headers
Installing private header files to /usr/include/

(lfs chroot) root:/sources/tcl8.6.10/unix# ln -sfv tclsh8.6 /usr/bin/tclsh
'/usr/bin/tclsh' -> 'tclsh8.6'

(lfs chroot) root:/sources/tcl8.6.10/unix# cd ../../

(lfs chroot) root:/sources# rm -rf tcl8.6.10/

(lfs chroot) root:/sources# 

```

<br>
# Expect-5.45.4

### INPUT
```
tar xf expect5.45.4.tar.gz
cd expect5.45.4/
./configure --prefix=/usr           \
            --with-tcl=/usr/lib     \
            --enable-shared         \
            --mandir=/usr/share/man \
            --with-tclinclude=/usr/include

```

### OUTPUT
```
===== TL;DR =====

checking sys/param.h presence... yes
checking for sys/param.h... yes
configure: creating ./config.status
config.status: creating Makefile

(lfs chroot) root:/sources/expect5.45.4# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
===== TL;DR =====

	-Wl,-rpath,/usr/lib \
	-Wl,-rpath,/usr/lib/expect5.45.4
: expect

real	0m0.767s
user	0m3.334s
sys	0m0.363s

(lfs chroot) root:/sources/expect5.45.4#

```

<br>
### INPUT
```
time make test

```

### OUTPUT
```
(lfs chroot) root:/sources/expect5.45.4# time make test
(echo 'if {![package vsatisfies [package provide Tcl] 8.6]} {return}' ; \
 echo 'package ifneeded Expect 5.45.4 \
    [list load [file join $dir libexpect5.45.4.so]]'\
) > pkgIndex.tcl
TCL_LIBRARY=`echo /usr/include/library` LD_LIBRARY_PATH=".:/usr/lib:" PATH=".:/usr/lib:/bin:/usr/bin:/sbin:/usr/sbin" TCLLIBPATH="." /usr/bin/tclsh8.6 `echo ./tests/all.tcl` 
cat.test
expect.test
logfile.test
via sendvia send_uservia send_stdoutvia send_ttypid.test
send.test
spawn.test
stty.test
all.tcl:	Total	29	Passed	29	Skipped	0	Failed	0
Sourced 0 Test Files.

real	0m13.284s
user	0m0.105s
sys	0m0.063s

(lfs chroot) root:/sources/expect5.45.4# 

```

<br>
### INPUT
```
time make install
ln -svf expect5.45.4/libexpect5.45.4.so /usr/lib
cd ../
rm -rf expect5.45.4/

```

### OUTPUT
```
===== TL;DR =====

    /usr/bin/install -c $i /usr/bin/$i ; \
    rm -f $i ; \
  else true; fi ; \
done

real	0m0.062s
user	0m0.125s
sys	0m0.023s

(lfs chroot) root:/sources/expect5.45.4# ln -svf expect5.45.4/libexpect5.45.4.so /usr/lib
'/usr/lib/libexpect5.45.4.so' -> 'expect5.45.4/libexpect5.45.4.so'

(lfs chroot) root:/sources/expect5.45.4# cd ../

(lfs chroot) root:/sources# rm -rf expect5.45.4/

(lfs chroot) root:/sources# 

```

<br>
# DejaGNU-1.6.2

### INPUT
```
tar xf dejagnu-1.6.2.tar.gz
cd dejagnu-1.6.2/
./configure --prefix=/usr
makeinfo --html --no-split -o doc/dejagnu.html doc/dejagnu.texi
makeinfo --plaintext       -o doc/dejagnu.txt  doc/dejagnu.texi

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf dejagnu-1.6.2.tar.gz

(lfs chroot) root:/sources# cd dejagnu-1.6.2/

(lfs chroot) root:/sources/dejagnu-1.6.2# ./configure --prefix=/usr
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

configure: creating ./config.status
config.status: creating Makefile
config.status: executing depfiles commands

(lfs chroot) root:/sources/dejagnu-1.6.2# makeinfo --html --no-split -o doc/dejagnu.html doc/dejagnu.texi

(lfs chroot) root:/sources/dejagnu-1.6.2# makeinfo --plaintext       -o doc/dejagnu.txt  doc/dejagnu.texi

(lfs chroot) root:/sources/dejagnu-1.6.2#

```

<br>
### INPUT
```
time make install
install -v -dm755  /usr/share/doc/dejagnu-1.6.2
install -v -m644   doc/dejagnu.{html,txt} /usr/share/doc/dejagnu-1.6.2

```

### OUTPUT
```
(lfs chroot) root:/sources/dejagnu-1.6.2# time make install
Done. Now run 'make install'.
make[1]: Entering directory '/sources/dejagnu-1.6.2'
 /bin/mkdir -p '/usr/bin'

===== TL;DR =====

 install-info --info-dir='/usr/share/info' '/usr/share/info/dejagnu.info'
make[1]: Leaving directory '/sources/dejagnu-1.6.2'

real	0m0.069s
user	0m0.062s
sys	0m0.008s

(lfs chroot) root:/sources/dejagnu-1.6.2# install -v -dm755  /usr/share/doc/dejagnu-1.6.2
install: creating directory '/usr/share/doc/dejagnu-1.6.2'

(lfs chroot) root:/sources/dejagnu-1.6.2# install -v -m644   doc/dejagnu.{html,txt} /usr/share/doc/dejagnu-1.6.2
'doc/dejagnu.html' -> '/usr/share/doc/dejagnu-1.6.2/dejagnu.html'
'doc/dejagnu.txt' -> '/usr/share/doc/dejagnu-1.6.2/dejagnu.txt'

(lfs chroot) root:/sources/dejagnu-1.6.2# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/dejagnu-1.6.2# time make check
Done. Now run 'make install'.
make  unit
make[1]: Entering directory '/sources/dejagnu-1.6.2'

===== TL;DR =====

Running ./testsuite/runtest.all/libs.exp ...
Running ./testsuite/runtest.all/load_lib.exp ...
Running ./testsuite/runtest.all/options.exp ...
Running ./testsuite/runtest.all/stats-sub.exp ...
Running ./testsuite/runtest.all/stats.exp ...

===== TL;DR =====

		=== runtest Summary ===

# of expected passes		77
DejaGnu version	1.6.2
Expect version	5.45.4
Tcl version	8.6

make[1]: Leaving directory '/sources/dejagnu-1.6.2'

real	0m2.291s
user	0m1.574s
sys	0m0.279s
(lfs chroot) root:/sources/dejagnu-1.6.2#

```

<br>
### INPUT
```
cd ../
rm -rf dejagnu-1.6.2/

```

### OUTPUT
```
(lfs chroot) root:/sources/dejagnu-1.6.2# cd ../

(lfs chroot) root:/sources# rm -rf dejagnu-1.6.2/

(lfs chroot) root:/sources# 

```

<br>
# Iana-Etc-20200821

### INPUT
```
tar xf iana-etc-20200821.tar.gz
cd iana-etc-20200821/
cp services protocols /etc/

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf iana-etc-20200821.tar.gz

(lfs chroot) root:/sources# cd iana-etc-20200821/

(lfs chroot) root:/sources/iana-etc-20200821# cp services protocols /etc/

(lfs chroot) root:/sources/iana-etc-20200821# 

```

<br>
### INPUT
```
cd ../
rm -rf iana-etc-20200821/

```

### OUTPUT
```
(lfs chroot) root:/sources/iana-etc-20200821# cd ../

(lfs chroot) root:/sources# rm -rf iana-etc-20200821/

(lfs chroot) root:/sources# 

```

<br>
# Glibc-2.32

### INPUT
```
tar xf glibc-2.32.tar.xz
cd glibc-2.32/
patch -Np1 -i ../glibc-2.32-fhs-1.patch
mkdir -v build
cd       build
../configure --prefix=/usr                            \
             --disable-werror                         \
             --enable-kernel=3.2                      \
             --enable-stack-protector=strong          \
             --with-headers=/usr/include              \
             libc_cv_slibdir=/lib

```

### OUTPUT
```

===== TL;DR =====
```

<br>
### INPUT
```
time make


```

### OUTPUT
```

===== TL;DR =====
```

<br>
### INPUT
```
case $(uname -m) in
  i?86)   ln -sfnv $PWD/elf/ld-linux.so.2        /lib ;;
  x86_64) ln -sfnv $PWD/elf/ld-linux-x86-64.so.2 /lib ;;
esac
time make check

```

### OUTPUT
```

===== TL;DR =====
```

<br>
### INPUT
```
touch /etc/ld.so.conf
sed '/test-installation/s@$(PERL)@echo not running@' -i ../Makefile
time make install
cp -v ../nscd/nscd.conf /etc/nscd.conf
mkdir -pv /var/cache/nscd
mkdir -pv /usr/lib/locale

```

### OUTPUT
```

===== TL;DR =====
```

<br>
### INPUT
```
localedef -i POSIX -f UTF-8 C.UTF-8 2> /dev/null || true
localedef -i cs_CZ -f UTF-8 cs_CZ.UTF-8
localedef -i de_DE -f ISO-8859-1 de_DE
localedef -i de_DE@euro -f ISO-8859-15 de_DE@euro
localedef -i de_DE -f UTF-8 de_DE.UTF-8
localedef -i el_GR -f ISO-8859-7 el_GR
localedef -i en_GB -f UTF-8 en_GB.UTF-8
localedef -i en_HK -f ISO-8859-1 en_HK
localedef -i en_PH -f ISO-8859-1 en_PH
localedef -i en_US -f ISO-8859-1 en_US
localedef -i en_US -f UTF-8 en_US.UTF-8
localedef -i es_MX -f ISO-8859-1 es_MX
localedef -i fa_IR -f UTF-8 fa_IR
localedef -i fr_FR -f ISO-8859-1 fr_FR
localedef -i fr_FR@euro -f ISO-8859-15 fr_FR@euro
localedef -i fr_FR -f UTF-8 fr_FR.UTF-8
localedef -i it_IT -f ISO-8859-1 it_IT
localedef -i it_IT -f UTF-8 it_IT.UTF-8
localedef -i ja_JP -f EUC-JP ja_JP
localedef -i ja_JP -f SHIFT_JIS ja_JP.SIJS 2> /dev/null || true
localedef -i ja_JP -f UTF-8 ja_JP.UTF-8
localedef -i ru_RU -f KOI8-R ru_RU.KOI8-R
localedef -i ru_RU -f UTF-8 ru_RU.UTF-8
localedef -i tr_TR -f UTF-8 tr_TR.UTF-8
localedef -i zh_CN -f GB18030 zh_CN.GB18030
localedef -i zh_HK -f BIG5-HKSCS zh_HK.BIG5-HKSCS
make localedata/install-locales

```

### OUTPUT
```

===== TL;DR =====
```

<br>
# Configuring Glibc

### INPUT
```
cat > /etc/nsswitch.conf << "EOF"
# Begin /etc/nsswitch.conf

passwd: files
group: files
shadow: files

hosts: files dns
networks: files

protocols: files
services: files
ethers: files
rpc: files

# End /etc/nsswitch.conf
EOF

```

### OUTPUT
```

===== TL;DR =====
```

<br>
### INPUT
```
tar -xf ../../tzdata2020a.tar.gz

ZONEINFO=/usr/share/zoneinfo
mkdir -pv $ZONEINFO/{posix,right}

for tz in etcetera southamerica northamerica europe africa antarctica  \
          asia australasia backward pacificnew systemv; do
    zic -L /dev/null   -d $ZONEINFO       ${tz}
    zic -L /dev/null   -d $ZONEINFO/posix ${tz}
    zic -L leapseconds -d $ZONEINFO/right ${tz}
done

cp -v zone.tab zone1970.tab iso3166.tab $ZONEINFO
zic -d $ZONEINFO -p America/New_York
unset ZONEINFO

tzselect

ln -sfv /usr/share/zoneinfo/<xxx> /etc/localtime

```

### OUTPUT
```

===== TL;DR =====
```

<br>
# Configuring the Dynamic Loader

### INPUT
```
cat > /etc/ld.so.conf << "EOF"
# Begin /etc/ld.so.conf
/usr/local/lib
/opt/lib

EOF

cat >> /etc/ld.so.conf << "EOF"
# Add an include directory
include /etc/ld.so.conf.d/*.conf

EOF
mkdir -pv /etc/ld.so.conf.d

```

### OUTPUT
```

===== TL;DR =====
```

<br>
#

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

<br>
### INPUT
```

cd ../
cd glibc-2.32/

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

* Create LFS-08.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-10.md)
[NEXT](index.md)
<br>

