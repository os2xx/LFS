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
(lfs chroot) root:/sources# tar xf glibc-2.32.tar.xz

(lfs chroot) root:/sources# cd glibc-2.32/

(lfs chroot) root:/sources/glibc-2.32# patch -Np1 -i ../glibc-2.32-fhs-1.patch
patching file Makeconfig
Hunk #1 succeeded at 245 (offset -5 lines).
patching file nscd/nscd.h

(lfs chroot) root:/sources/glibc-2.32# mkdir -v build
mkdir: created directory 'build'

(lfs chroot) root:/sources/glibc-2.32# cd       build

(lfs chroot) root:/sources/glibc-2.32/build# ../configure --prefix=/usr                            \
>              --disable-werror                         \
>              --enable-kernel=3.2                      \
>              --enable-stack-protector=strong          \

===== TL;DR =====

config.status: creating Makefile
config.status: creating config.h
config.status: executing default commands

(lfs chroot) root:/sources/glibc-2.32/build#

```

<br>
### INPUT
```
time make

```

### OUTPUT
```

===== TL;DR =====
make[2]: Leaving directory '/sources/glibc-2.32/elf'
make[1]: Leaving directory '/sources/glibc-2.32'

real	2m35.656s
user	9m45.811s
sys	2m30.884s

(lfs chroot) root:/sources/glibc-2.32/build# 

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

UNSUPPORTED: resolv/tst-resolv-ai_idn
UNSUPPORTED: resolv/tst-resolv-ai_idn-latin1
UNSUPPORTED: stdlib/tst-system
UNSUPPORTED: string/tst-strerror
UNSUPPORTED: string/tst-strsignal
Summary of test results:
      3 FAIL
   4233 PASS
     28 UNSUPPORTED
     17 XFAIL
      2 XPASS
make[1]: *** [Makefile:633: tests] Error 1
make[1]: Leaving directory '/sources/glibc-2.32'
make: *** [Makefile:9: check] Error 2

real	17m42.348s
user	26m39.834s
sys	5m16.890s

(lfs chroot) root:/sources/glibc-2.32/build# 

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

  /sources/glibc-2.32/build/elf/ldconfig  \
			/lib /usr/lib
LD_SO=ld-linux-x86-64.so.2 CC="gcc" echo not running scripts/test-installation.pl /sources/glibc-2.32/build/
not running scripts/test-installation.pl /sources/glibc-2.32/build/
make[1]: Leaving directory '/sources/glibc-2.32'

real	0m28.986s
user	0m37.396s
sys	0m5.469s

(lfs chroot) root:/sources/glibc-2.32/build# cp -v ../nscd/nscd.conf /etc/nscd.conf
'../nscd/nscd.conf' -> '/etc/nscd.conf'

(lfs chroot) root:/sources/glibc-2.32/build# mkdir -pv /var/cache/nscd
mkdir: created directory '/var/cache/nscd'

(lfs chroot) root:/sources/glibc-2.32/build# mkdir -pv /usr/lib/locale
mkdir: created directory '/usr/lib/locale'

(lfs chroot) root:/sources/glibc-2.32/build# 

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

zh_SG.GB2312... done
 done
zh_TW.UTF-8...zh_TW.EUC-TW... done
 done
zh_TW.BIG5...zu_ZA.UTF-8... done
zu_ZA.ISO-8859-1... done
 done
 done
 done
 done
 done
make[2]: Leaving directory '/sources/glibc-2.32/localedata'
make[1]: Leaving directory '/sources/glibc-2.32'

(lfs chroot) root:/sources/glibc-2.32/build#

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
(lfs chroot) root:/sources/glibc-2.32/build# cat > /etc/nsswitch.conf << "EOF"
> # Begin /etc/nsswitch.conf
> 
> passwd: files
> group: files
> shadow: files
> 
> hosts: files dns
> networks: files
> 
> protocols: files
> services: files
> ethers: files
> rpc: files
> 
> # End /etc/nsswitch.conf
> EOF

(lfs chroot) root:/sources/glibc-2.32/build# 

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

```

### OUTPUT
```
(lfs chroot) root:/sources/glibc-2.32/build# tar -xf ../../tzdata2020a.tar.gz

(lfs chroot) root:/sources/glibc-2.32/build# ZONEINFO=/usr/share/zoneinfo

(lfs chroot) root:/sources/glibc-2.32/build# mkdir -pv $ZONEINFO/{posix,right}
mkdir: created directory '/usr/share/zoneinfo/posix'
mkdir: created directory '/usr/share/zoneinfo/right'

(lfs chroot) root:/sources/glibc-2.32/build# for tz in etcetera southamerica northamerica europe africa antarctica  \
>           asia australasia backward pacificnew systemv; do
>     zic -L /dev/null   -d $ZONEINFO       ${tz}
>     zic -L /dev/null   -d $ZONEINFO/posix ${tz}
>     zic -L leapseconds -d $ZONEINFO/right ${tz}

===== TL;DR =====

warning: "leapseconds", line 79: "#expires" is obsolescent; use "Expires"
warning: "leapseconds", line 79: "#expires" is obsolescent; use "Expires"
warning: "leapseconds", line 79: "#expires" is obsolescent; use "Expires"

(lfs chroot) root:/sources/glibc-2.32/build# cp -v zone.tab zone1970.tab iso3166.tab $ZONEINFO
'zone.tab' -> '/usr/share/zoneinfo/zone.tab'
'zone1970.tab' -> '/usr/share/zoneinfo/zone1970.tab'
'iso3166.tab' -> '/usr/share/zoneinfo/iso3166.tab'

(lfs chroot) root:/sources/glibc-2.32/build# zic -d $ZONEINFO -p America/New_York

(lfs chroot) root:/sources/glibc-2.32/build# unset ZONEINFO

(lfs chroot) root:/sources/glibc-2.32/build# tzselect
Please identify a location so that time zone rules can be set correctly.
Please select a continent, ocean, "coord", or "TZ".
 1) Africa
 2) Americas
 3) Antarctica
 4) Asia
 5) Atlantic Ocean
 6) Australia
 7) Europe
 8) Indian Ocean
 9) Pacific Ocean
10) coord - I want to use geographical coordinates.
11) TZ - I want to specify the timezone using the Posix TZ format.
#? 4

Please select a country whose clocks agree with yours.
 1) Afghanistan		  14) India		    27) Lebanon		      40) Singapore
 2) Armenia		  15) Indonesia		    28) Macau		      41) Sri Lanka
 3) Azerbaijan		  16) Iran		    29) Malaysia	      42) Syria
 4) Bahrain		  17) Iraq		    30) Mongolia	      43) Taiwan
 5) Bangladesh		  18) Israel		    31) Myanmar (Burma)	      44) Tajikistan
 6) Bhutan		  19) Japan		    32) Nepal		      45) Thailand
 7) Brunei		  20) Jordan		    33) Oman		      46) Turkmenistan
 8) Cambodia		  21) Kazakhstan	    34) Pakistan	      47) United Arab Emirates
 9) China		  22) Korea (North)	    35) Palestine	      48) Uzbekistan
10) Cyprus		  23) Korea (South)	    36) Philippines	      49) Vietnam
11) East Timor		  24) Kuwait		    37) Qatar		      50) Yemen
12) Georgia		  25) Kyrgyzstan	    38) Russia
13) Hong Kong		  26) Laos		    39) Saudi Arabia
#? 15

Please select one of the following timezones.
1) Java, Sumatra
2) Borneo (west, central)
3) Borneo (east, south); Sulawesi/Celebes, Bali, Nusa Tengarra; Timor (west)
4) New Guinea (West Papua / Irian Jaya); Malukus/Moluccas
#? 1

The following information has been given:

	Indonesia
	Java, Sumatra

Therefore TZ='Asia/Jakarta' will be used.
Selected time is now:	Sun Dec  6 16:11:54 WIB 2020.
Universal Time is now:	Sun Dec  6 09:11:54 UTC 2020.
Is the above information OK?
1) Yes
2) No
#? 1

You can make this change permanent for yourself by appending the line
	TZ='Asia/Jakarta'; export TZ
to the file '.profile' in your home directory; then log out and log in again.

Here is that TZ value again, this time on standard output so that you
can use the /usr/bin/tzselect command in shell scripts:
Asia/Jakarta

(lfs chroot) root:/sources/glibc-2.32/build#

```

### INPUT
```
ln -sfv /usr/share/zoneinfo/Asia/Jakarta /etc/localtime

```

### OUTPUT
```
(lfs chroot) root:/sources/glibc-2.32/build# ln -sfv /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
'/etc/localtime' -> '/usr/share/zoneinfo/Asia/Jakarta'

(lfs chroot) root:/sources/glibc-2.32/build# 

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
(lfs chroot) root:/sources/glibc-2.32/build# cat > /etc/ld.so.conf << "EOF"
> # Begin /etc/ld.so.conf
> /usr/local/lib
> /opt/lib
> 
> EOF

(lfs chroot) root:/sources/glibc-2.32/build# cat >> /etc/ld.so.conf << "EOF"
> # Add an include directory
> include /etc/ld.so.conf.d/*.conf
> 
> EOF

(lfs chroot) root:/sources/glibc-2.32/build# mkdir -pv /etc/ld.so.conf.d
mkdir: created directory '/etc/ld.so.conf.d'

(lfs chroot) root:/sources/glibc-2.32/build# 

```

<br>
### INPUT
```
cd ../../
rm -rf glibc-2.32/

```

### OUTPUT
```
(lfs chroot) root:/sources/glibc-2.32/build# cd ../../

(lfs chroot) root:/sources# rm -rf glibc-2.32/

(lfs chroot) root:/sources# 

```

<br>
# Zlib-1.2.11

### INPUT
```
tar xf zlib-1.2.11.tar.xz
cd zlib-1.2.11/
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf zlib-1.2.11.tar.xz

(lfs chroot) root:/sources# cd zlib-1.2.11/

(lfs chroot) root:/sources/zlib-1.2.11# ./configure --prefix=/usr
Checking for gcc...
Checking for shared library support...
Building shared library libz.so.1.2.11 with gcc.
Checking for size_t... Yes.
Checking for off64_t... Yes.
Checking for fseeko... Yes.
Checking for strerror... Yes.
Checking for unistd.h... Yes.
Checking for stdarg.h... Yes.
Checking whether to use vs[n]printf() or s[n]printf()... using vs[n]printf().
Checking for vsnprintf() in stdio.h... Yes.
Checking for return value of vsnprintf()... Yes.
Checking for attribute(visibility) support... Yes.

(lfs chroot) root:/sources/zlib-1.2.11# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/zlib-1.2.11# time make
gcc -O3 -D_LARGEFILE64_SOURCE=1 -DHAVE_HIDDEN -I. -c -o example.o test/example.c
gcc -O3 -D_LARGEFILE64_SOURCE=1 -DHAVE_HIDDEN  -c -o adler32.o adler32.c
gcc -O3 -D_LARGEFILE64_SOURCE=1 -DHAVE_HIDDEN  -c -o crc32.o crc32.c

===== TL;DR =====

real	0m1.280s
user	0m6.074s
sys	0m0.660s

(lfs chroot) root:/sources/zlib-1.2.11# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/zlib-1.2.11# time make check
hello world
hello world
hello world
zlib version 1.2.11 = 0x12b0, compile flags = 0xa9
uncompress(): hello, hello!
zlib version 1.2.11 = 0x12b0, compile flags = 0xa9
uncompress(): hello, hello!
gzread(): hello, hello!
gzgets() after gzseek:  hello!
gzread(): hello, hello!
gzgets() after gzseek:  hello!
inflate(): hello, hello!
inflate(): hello, hello!
zlib version 1.2.11 = 0x12b0, compile flags = 0xa9
large_inflate(): OK
after inflateSync(): hello, hello!
uncompress(): hello, hello!
inflate with dictionary: hello, hello!
		*** zlib shared test OK ***
gzread(): hello, hello!
gzgets() after gzseek:  hello!
inflate(): hello, hello!
large_inflate(): OK
large_inflate(): OK
after inflateSync(): hello, hello!
after inflateSync(): hello, hello!
inflate with dictionary: hello, hello!
inflate with dictionary: hello, hello!
		*** zlib 64-bit test OK ***
		*** zlib test OK ***

real	0m0.024s
user	0m0.018s
sys	0m0.058s

(lfs chroot) root:/sources/zlib-1.2.11# 

```

<br>
### INPUT
```
time make install
mv -v /usr/lib/libz.so.* /lib
ln -sfv ../../lib/$(readlink /usr/lib/libz.so) /usr/lib/libz.so

cd ../
rm -rf zlib-1.2.11/

```

### OUTPUT
```
(lfs chroot) root:/sources/zlib-1.2.11# time make install
rm -f /usr/lib/libz.a
cp libz.a /usr/lib
chmod 644 /usr/lib/libz.a
cp libz.so.1.2.11 /usr/lib
chmod 755 /usr/lib/libz.so.1.2.11
rm -f /usr/share/man/man3/zlib.3
cp zlib.3 /usr/share/man/man3
chmod 644 /usr/share/man/man3/zlib.3
rm -f /usr/lib/pkgconfig/zlib.pc
cp zlib.pc /usr/lib/pkgconfig
chmod 644 /usr/lib/pkgconfig/zlib.pc
rm -f /usr/include/zlib.h /usr/include/zconf.h
cp zlib.h zconf.h /usr/include
chmod 644 /usr/include/zlib.h /usr/include/zconf.h

real	0m0.625s
user	0m0.020s
sys	0m0.004s

(lfs chroot) root:/sources/zlib-1.2.11# mv -v /usr/lib/libz.so.* /lib
renamed '/usr/lib/libz.so.1' -> '/lib/libz.so.1'
renamed '/usr/lib/libz.so.1.2.11' -> '/lib/libz.so.1.2.11'

(lfs chroot) root:/sources/zlib-1.2.11# ln -sfv ../../lib/$(readlink /usr/lib/libz.so) /usr/lib/libz.so
'/usr/lib/libz.so' -> '../../lib/libz.so.1.2.11'

(lfs chroot) root:/sources/zlib-1.2.11# cd ../

(lfs chroot) root:/sources# rm -rf zlib-1.2.11/

(lfs chroot) root:/sources# 

```

<br>
# Bzip2-1.0.8

### INPUT
```
tar xf bzip2-1.0.8.tar.gz
cd bzip2-1.0.8/
patch -Np1 -i ../bzip2-1.0.8-install_docs-1.patch
sed -i 's@\(ln -s -f \)$(PREFIX)/bin/@\1@' Makefile
sed -i "s@(PREFIX)/man@(PREFIX)/share/man@g" Makefile
make -f Makefile-libbz2_so
make clean

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf bzip2-1.0.8.tar.gz

(lfs chroot) root:/sources# cd bzip2-1.0.8/

(lfs chroot) root:/sources/bzip2-1.0.8# patch -Np1 -i ../bzip2-1.0.8-install_docs-1.patch
patching file Makefile

(lfs chroot) root:/sources/bzip2-1.0.8# sed -i 's@\(ln -s -f \)$(PREFIX)/bin/@\1@' Makefile

(lfs chroot) root:/sources/bzip2-1.0.8# sed -i "s@(PREFIX)/man@(PREFIX)/share/man@g" Makefile

(lfs chroot) root:/sources/bzip2-1.0.8# make -f Makefile-libbz2_so
gcc -fpic -fPIC -Wall -Winline -O2 -g -D_FILE_OFFSET_BITS=64 -c blocksort.c
gcc -fpic -fPIC -Wall -Winline -O2 -g -D_FILE_OFFSET_BITS=64 -c huffman.c
gcc -fpic -fPIC -Wall -Winline -O2 -g -D_FILE_OFFSET_BITS=64 -c crctable.c

===== TL;DR =====

gcc -fpic -fPIC -Wall -Winline -O2 -g -D_FILE_OFFSET_BITS=64 -o bzip2-shared bzip2.c libbz2.so.1.0.8
rm -f libbz2.so.1.0
ln -s libbz2.so.1.0.8 libbz2.so.1.0

(lfs chroot) root:/sources/bzip2-1.0.8# make clean
rm -f *.o libbz2.a bzip2 bzip2recover \
sample1.rb2 sample2.rb2 sample3.rb2 \
sample1.tst sample2.tst sample3.tst

(lfs chroot) root:/sources/bzip2-1.0.8# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/bzip2-1.0.8# time make
gcc -Wall -Winline -O2 -g -D_FILE_OFFSET_BITS=64 -c huffman.c

If compilation produces errors, or a large number of warnings,
please read README.COMPILATION.PROBLEMS -- you might be able to
adjust the flags in this Makefile to improve matters.

===== TL;DR =====

real	0m1.211s
user	0m3.037s
sys	0m0.199s

(lfs chroot) root:/sources/bzip2-1.0.8#

```

<br>
### INPUT
```
time make PREFIX=/usr install
cp -v bzip2-shared /bin/bzip2
cp -av libbz2.so* /lib
ln -sv ../../lib/libbz2.so.1.0 /usr/lib/libbz2.so
rm -v /usr/bin/{bunzip2,bzcat,bzip2}
ln -sv bzip2 /bin/bunzip2
ln -sv bzip2 /bin/bzcat

cd ../
rm -rf bzip2-1.0.8/

```

### OUTPUT
```
(lfs chroot) root:/sources/bzip2-1.0.8# time make PREFIX=/usr install
if ( test ! -d /usr/bin ) ; then mkdir -p /usr/bin ; fi
if ( test ! -d /usr/lib ) ; then mkdir -p /usr/lib ; fi
if ( test ! -d /usr/share/man ) ; then mkdir -p /usr/share/man ; fi

===== TL;DR =====

real	0m0.142s
user	0m0.076s
sys	0m0.043s

(lfs chroot) root:/sources/bzip2-1.0.8# cp -v bzip2-shared /bin/bzip2
'bzip2-shared' -> '/bin/bzip2'

(lfs chroot) root:/sources/bzip2-1.0.8# cp -av libbz2.so* /lib
'libbz2.so.1.0' -> '/lib/libbz2.so.1.0'
'libbz2.so.1.0.8' -> '/lib/libbz2.so.1.0.8'

(lfs chroot) root:/sources/bzip2-1.0.8# ln -sv ../../lib/libbz2.so.1.0 /usr/lib/libbz2.so
'/usr/lib/libbz2.so' -> '../../lib/libbz2.so.1.0'

(lfs chroot) root:/sources/bzip2-1.0.8# rm -v /usr/bin/{bunzip2,bzcat,bzip2}
removed '/usr/bin/bunzip2'
removed '/usr/bin/bzcat'
removed '/usr/bin/bzip2'

(lfs chroot) root:/sources/bzip2-1.0.8# ln -sv bzip2 /bin/bunzip2
'/bin/bunzip2' -> 'bzip2'

(lfs chroot) root:/sources/bzip2-1.0.8# ln -sv bzip2 /bin/bzcat
'/bin/bzcat' -> 'bzip2'

(lfs chroot) root:/sources/bzip2-1.0.8# cd ../

(lfs chroot) root:/sources# rm -rf bzip2-1.0.8/

(lfs chroot) root:/sources# 

```

<br>
# Xz-5.2.5

### INPUT
```
tar xf xz-5.2.5.tar.xz
cd xz-5.2.5/
./configure --prefix=/usr    \
            --disable-static \
            --docdir=/usr/share/doc/xz-5.2.5

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf xz-5.2.5.tar.xz

(lfs chroot) root:/sources# cd xz-5.2.5/

(lfs chroot) root:/sources/xz-5.2.5# ./configure --prefix=/usr    \
>             --disable-static \
>             --docdir=/usr/share/doc/xz-5.2.5

===== TL;DR =====

config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

(lfs chroot) root:/sources/xz-5.2.5#

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/xz-5.2.5# time make
make  all-recursive
make[1]: Entering directory '/sources/xz-5.2.5'
Making all in src

===== TL;DR =====

make[2]: Entering directory '/sources/xz-5.2.5'
make[2]: Leaving directory '/sources/xz-5.2.5'
make[1]: Leaving directory '/sources/xz-5.2.5'

real	0m3.195s
user	0m11.290s
sys	0m1.975s

(lfs chroot) root:/sources/xz-5.2.5#

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/xz-5.2.5# time make check
Making check in src
make[1]: Entering directory '/sources/xz-5.2.5/src'
Making check in liblzma
make[2]: Entering directory '/sources/xz-5.2.5/src/liblzma'

===== TL;DR =====

==================
All 9 tests passed
==================
make[2]: Leaving directory '/sources/xz-5.2.5/tests'
make[1]: Leaving directory '/sources/xz-5.2.5/tests'
make[1]: Entering directory '/sources/xz-5.2.5'
make[1]: Leaving directory '/sources/xz-5.2.5'

real	0m7.546s
user	0m7.816s
sys	0m1.844s

(lfs chroot) root:/sources/xz-5.2.5#

```

<br>
### INPUT
```
time make install
mv -v   /usr/bin/{lzma,unlzma,lzcat,xz,unxz,xzcat} /bin
mv -v /usr/lib/liblzma.so.* /lib
ln -svf ../../lib/$(readlink /usr/lib/liblzma.so) /usr/lib/liblzma.so
cd ../
rm -rf xz-5.2.5/

```

### OUTPUT
```
(lfs chroot) root:/sources/xz-5.2.5# time make install
Making install in src
make[1]: Entering directory '/sources/xz-5.2.5/src'
Making install in liblzma

===== TL;DR =====

make[2]: Leaving directory '/sources/xz-5.2.5'
make[1]: Leaving directory '/sources/xz-5.2.5'

real	0m0.630s
user	0m0.725s
sys	0m0.120s

(lfs chroot) root:/sources/xz-5.2.5# mv -v   /usr/bin/{lzma,unlzma,lzcat,xz,unxz,xzcat} /bin
renamed '/usr/bin/lzma' -> '/bin/lzma'
renamed '/usr/bin/unlzma' -> '/bin/unlzma'
renamed '/usr/bin/lzcat' -> '/bin/lzcat'
renamed '/usr/bin/xz' -> '/bin/xz'
renamed '/usr/bin/unxz' -> '/bin/unxz'
renamed '/usr/bin/xzcat' -> '/bin/xzcat'

(lfs chroot) root:/sources/xz-5.2.5# mv -v /usr/lib/liblzma.so.* /lib
renamed '/usr/lib/liblzma.so.5' -> '/lib/liblzma.so.5'
renamed '/usr/lib/liblzma.so.5.2.5' -> '/lib/liblzma.so.5.2.5'

(lfs chroot) root:/sources/xz-5.2.5# ln -svf ../../lib/$(readlink /usr/lib/liblzma.so) /usr/lib/liblzma.so
'/usr/lib/liblzma.so' -> '../../lib/liblzma.so.5.2.5'

(lfs chroot) root:/sources/xz-5.2.5# cd ../

(lfs chroot) root:/sources# rm -rf xz-5.2.5/

(lfs chroot) root:/sources# 

```

<br>
# Zstd-1.4.5

### INPUT
```
tar xf zstd-1.4.5.tar.gz
cd zstd-1.4.5/
time make

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf zstd-1.4.5.tar.gz

(lfs chroot) root:/sources# cd zstd-1.4.5/

(lfs chroot) root:/sources/zstd-1.4.5# time make
make[1]: Entering directory '/sources/zstd-1.4.5/lib'
make[1]: Entering directory '/sources/zstd-1.4.5/programs'
make[1]: Nothing to be done for 'lib-release'.

===== TL;DR =====

cp programs/zstd .

real	0m25.783s
user	0m24.555s
sys	0m0.657s

(lfs chroot) root:/sources/zstd-1.4.5#

```

<br>
### INPUT
```
make prefix=/usr install
rm -v /usr/lib/libzstd.a
mv -v /usr/lib/libzstd.so.* /lib
ln -sfv ../../lib/$(readlink /usr/lib/libzstd.so) /usr/lib/libzstd.so
cd ../
rm -rf zstd-1.4.5/

```

### OUTPUT
```
(lfs chroot) root:/sources/zstd-1.4.5# make prefix=/usr install
make[1]: Entering directory '/sources/zstd-1.4.5/lib'
cc -Wall -Wextra -Wcast-qual -Wcast-align -Wshadow -Wstrict-aliasing=1 

===== TL;DR =====

Installing binaries
Installing man pages
zstd installation completed
make[1]: Leaving directory '/sources/zstd-1.4.5/programs'

(lfs chroot) root:/sources/zstd-1.4.5# rm -v /usr/lib/libzstd.a
removed '/usr/lib/libzstd.a'

(lfs chroot) root:/sources/zstd-1.4.5# mv -v /usr/lib/libzstd.so.* /lib
renamed '/usr/lib/libzstd.so.1' -> '/lib/libzstd.so.1'
renamed '/usr/lib/libzstd.so.1.4.5' -> '/lib/libzstd.so.1.4.5'

(lfs chroot) root:/sources/zstd-1.4.5# ln -sfv ../../lib/$(readlink /usr/lib/libzstd.so) /usr/lib/libzstd.so
'/usr/lib/libzstd.so' -> '../../lib/libzstd.so.1.4.5'

(lfs chroot) root:/sources/zstd-1.4.5# cd ../

(lfs chroot) root:/sources# rm -rf zstd-1.4.5/

(lfs chroot) root:/sources#

```

<br>
# File-5.39

### INPUT
```
tar xf file-5.39.tar.gz
cd file-5.39/
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf file-5.39.tar.gz

(lfs chroot) root:/sources# cd file-5.39/

(lfs chroot) root:/sources/file-5.39# ./configure --prefix=/usr
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: creating config.h
config.status: executing depfiles commands
config.status: executing libtool commands

(lfs chroot) root:/sources/file-5.39# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/file-5.39# time make
make  all-recursive
make[1]: Entering directory '/sources/file-5.39'
Making all in src
make[2]: Entering directory '/sources/file-5.39/src'

===== TL;DR =====

real	0m1.881s
user	0m6.464s
sys	0m0.862s

(lfs chroot) root:/sources/file-5.39# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/file-5.39# time make check
Making check in src
make[1]: Entering directory '/sources/file-5.39/src'
make  check-am
make[2]: Entering directory '/sources/file-5.39/src'

===== TL;DR =====

real	0m0.421s
user	0m0.382s
sys	0m0.051s

(lfs chroot) root:/sources/file-5.39#

```

<br>
### INPUT
```
time make install
cd ../
rm -rf file-5.39/

```

### OUTPUT
```
(lfs chroot) root:/sources/file-5.39# time make install
Making install in src
make[1]: Entering directory '/sources/file-5.39/src'
make  install-am
make[2]: Entering directory '/sources/file-5.39/src'

===== TL;DR =====

real	0m0.259s
user	0m0.156s
sys	0m0.036s

(lfs chroot) root:/sources/file-5.39# cd ../

(lfs chroot) root:/sources# rm -rf file-5.39/

(lfs chroot) root:/sources# 

```

<br>
# Readline-8.0

### INPUT
```
tar xf readline-8.0.tar.gz
cd readline-8.0/
sed -i '/MV.*old/d' Makefile.in
sed -i '/{OLDSUFF}/c:' support/shlib-install
./configure --prefix=/usr    \
            --disable-static \
            --with-curses    \
            --docdir=/usr/share/doc/readline-8.0

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf readline-8.0.tar.gz

(lfs chroot) root:/sources# cd readline-8.0/

(lfs chroot) root:/sources/readline-8.0# sed -i '/MV.*old/d' Makefile.in

(lfs chroot) root:/sources/readline-8.0# sed -i '/{OLDSUFF}/c:' support/shlib-install

(lfs chroot) root:/sources/readline-8.0# ./configure --prefix=/usr    \
>             --disable-static \
>             --with-curses    \
>             --docdir=/usr/share/doc/readline-8.0

===== TL;DR =====

config.status: creating readline.pc
config.status: creating config.h
config.status: executing default commands

(lfs chroot) root:/sources/readline-8.0# 

```

<br>
### INPUT
```
time make SHLIB_LIBS="-lncursesw"

```

### OUTPUT
```
(lfs chroot) root:/sources/readline-8.0# time make SHLIB_LIBS="-lncursesw"
test -d shlib || mkdir shlib
( cd shlib ; make -j6 --jobserver-auth=3,4 all )
make[1]: warning: -j6 forced in submake: resetting jobserver mode.

===== TL;DR =====

real	0m1.068s
user	0m4.845s
sys	0m0.685s

(lfs chroot) root:/sources/readline-8.0# 

```

<br>
### INPUT
```
time make SHLIB_LIBS="-lncursesw" install
mv -v /usr/lib/lib{readline,history}.so.* /lib
chmod -v u+w /lib/lib{readline,history}.so.*
ln -sfv ../../lib/$(readlink /usr/lib/libreadline.so) /usr/lib/libreadline.so
ln -sfv ../../lib/$(readlink /usr/lib/libhistory.so ) /usr/lib/libhistory.so
install -v -m644 doc/*.{ps,pdf,html,dvi} /usr/share/doc/readline-8.0
cd ../
rm -rf readline-8.0/

```

### OUTPUT
```
(lfs chroot) root:/sources/readline-8.0# time make SHLIB_LIBS="-lncursesw" install
/bin/sh ./support/mkinstalldirs /usr/include \
	/usr/include/readline /usr/lib \
	/usr/share/info /usr/share/man/man3 /usr/share/doc/readline-8.0 \
	/usr/lib/pkgconfig

===== TL;DR =====

real	0m0.125s
user	0m0.060s
sys	0m0.011s

(lfs chroot) root:/sources/readline-8.0# mv -v /usr/lib/lib{readline,history}.so.* /lib
renamed '/usr/lib/libreadline.so.8' -> '/lib/libreadline.so.8'
renamed '/usr/lib/libreadline.so.8.0' -> '/lib/libreadline.so.8.0'
renamed '/usr/lib/libhistory.so.8' -> '/lib/libhistory.so.8'
renamed '/usr/lib/libhistory.so.8.0' -> '/lib/libhistory.so.8.0'

(lfs chroot) root:/sources/readline-8.0# chmod -v u+w /lib/lib{readline,history}.so.*
mode of '/lib/libreadline.so.8' retained as 0755 (rwxr-xr-x)
mode of '/lib/libreadline.so.8.0' retained as 0755 (rwxr-xr-x)
mode of '/lib/libhistory.so.8' retained as 0755 (rwxr-xr-x)
mode of '/lib/libhistory.so.8.0' retained as 0755 (rwxr-xr-x)

(lfs chroot) root:/sources/readline-8.0# ln -sfv ../../lib/$(readlink /usr/lib/libreadline.so) /usr/lib/libreadline.so
'/usr/lib/libreadline.so' -> '../../lib/libreadline.so.8'

(lfs chroot) root:/sources/readline-8.0# ln -sfv ../../lib/$(readlink /usr/lib/libhistory.so ) /usr/lib/libhistory.so
'/usr/lib/libhistory.so' -> '../../lib/libhistory.so.8'

(lfs chroot) root:/sources/readline-8.0# install -v -m644 doc/*.{ps,pdf,html,dvi} /usr/share/doc/readline-8.0
'doc/history.ps' -> '/usr/share/doc/readline-8.0/history.ps'
'doc/history_3.ps' -> '/usr/share/doc/readline-8.0/history_3.ps'
'doc/readline.ps' -> '/usr/share/doc/readline-8.0/readline.ps'

===== TL;DR =====

'doc/history.dvi' -> '/usr/share/doc/readline-8.0/history.dvi'
'doc/readline.dvi' -> '/usr/share/doc/readline-8.0/readline.dvi'
'doc/rluserman.dvi' -> '/usr/share/doc/readline-8.0/rluserman.dvi'

(lfs chroot) root:/sources/readline-8.0# cd ../

(lfs chroot) root:/sources# rm -rf readline-8.0/

(lfs chroot) root:/sources# 

```

<br>
# M4-1.4.18

### INPUT
```
tar xf m4-1.4.18.tar.xz
cd m4-1.4.18/
sed -i 's/IO_ftrylockfile/IO_EOF_SEEN/' lib/*.c
echo "#define _IO_IN_BACKUP 0x100" >> lib/stdio-impl.h
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf m4-1.4.18.tar.xz

(lfs chroot) root:/sources# cd m4-1.4.18/

(lfs chroot) root:/sources/m4-1.4.18# sed -i 's/IO_ftrylockfile/IO_EOF_SEEN/' lib/*.c

(lfs chroot) root:/sources/m4-1.4.18# echo "#define _IO_IN_BACKUP 0x100" >> lib/stdio-impl.h

(lfs chroot) root:/sources/m4-1.4.18# ./configure --prefix=/usr

checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: creating lib/config.h
config.status: executing depfiles commands
config.status: executing stamp-h commands

(lfs chroot) root:/sources/m4-1.4.18# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/m4-1.4.18# time make
make  all-recursive
make[1]: Entering directory '/sources/m4-1.4.18'
Making all in .
make[2]: Entering directory '/sources/m4-1.4.18'

===== TL;DR =====

real	0m2.096s
user	0m6.527s
sys	0m1.192s

(lfs chroot) root:/sources/m4-1.4.18# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/m4-1.4.18# time make check
  GEN      public-submodule-commit
make  check-recursive
make[1]: Entering directory '/sources/m4-1.4.18'
Making check in .

===== TL;DR =====

============================================================================
Testsuite summary for GNU M4 1.4.18
============================================================================
# TOTAL: 170
# PASS:  157
# SKIP:  13
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================
make[6]: Leaving directory '/sources/m4-1.4.18/tests'
make[5]: Leaving directory '/sources/m4-1.4.18/tests'
make[4]: Leaving directory '/sources/m4-1.4.18/tests'
make[3]: Leaving directory '/sources/m4-1.4.18/tests'
make[2]: Leaving directory '/sources/m4-1.4.18/tests'
make[1]: Leaving directory '/sources/m4-1.4.18'

real	0m11.022s
user	0m22.038s
sys	0m4.207s

(lfs chroot) root:/sources/m4-1.4.18# 

```

<br>
### INPUT
```
time make install

cd ../
rm -rf m4-1.4.18/

```

### OUTPUT
```
(lfs chroot) root:/sources/m4-1.4.18# time make install
make  install-recursive
make[1]: Entering directory '/sources/m4-1.4.18'
Making install in .

===== TL;DR =====

real	0m0.373s
user	0m0.348s
sys	0m0.026s

(lfs chroot) root:/sources/m4-1.4.18# cd ../

(lfs chroot) root:/sources# rm -rf m4-1.4.18/

(lfs chroot) root:/sources# 

```

<br>
# Bc-3.1.5 

### INPUT
```
tar xf bc-3.1.5.tar.xz
cd bc-3.1.5/
PREFIX=/usr CC=gcc CFLAGS="-std=c99" ./configure.sh -G -O3

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf bc-3.1.5.tar.xz

(lfs chroot) root:/sources# cd bc-3.1.5/

(lfs chroot) root:/sources/bc-3.1.5# PREFIX=/usr CC=gcc CFLAGS="-std=c99" ./configure.sh -G -O3
Testing NLS...
NLS works.

Testing gencat...
gencat works.

Testing history...
History works.

Building bc
Building dc

===== TL;DR =====

LONG_BIT=
GEN_HOST=1
GEN_EMU=

(lfs chroot) root:/sources/bc-3.1.5# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/bc-3.1.5# time make
mkdir -p bin
gcc -std=c99 -o gen/strgen gen/strgen.c
gcc -DBC_ENABLED=1 -DDC_ENABLED=1 -I./include/ -DVERSION=3.1.5  -DEXECPREFIX= -DMAINEXEC=bc 

===== TL;DR =====

real	0m1.358s
user	0m3.315s
sys	0m0.271s
(lfs chroot) root:/sources/bc-3.1.5# 

```

<br>
### INPUT
```
time make test

```

### OUTPUT
```
(lfs chroot) root:/sources/bc-3.1.5# time make test
***********************************************************************

===== TL;DR =====

All dc tests passed.

===== TL;DR =====

All bc tests passed.

***********************************************************************

real	0m1.696s
user	0m1.842s
sys	0m0.539s

(lfs chroot) root:/sources/bc-3.1.5# 

```

<br>
### INPUT
```
time make install

cd ../
rm -rf bc-3.1.5/

```

### OUTPUT
```
(lfs chroot) root:/sources/bc-3.1.5# time make install
./locale_install.sh /usr/share/locale/%L/%N bc 
./safe-install.sh -Dm644 manuals/bc.1 /usr/share/man/man1/bc.1
./safe-install.sh -Dm644 manuals/dc.1 /usr/share/man/man1/dc.1
./install.sh /usr/bin ""

real	0m10.579s
user	0m10.435s
sys	0m0.265s

(lfs chroot) root:/sources/bc-3.1.5# cd ../

(lfs chroot) root:/sources# rm -rf bc-3.1.5/

(lfs chroot) root:/sources# 

```

<br>
# Flex-2.6.4

### INPUT
```
tar xf flex-2.6.4.tar.gz
cd flex-2.6.4/
./configure --prefix=/usr --docdir=/usr/share/doc/flex-2.6.4

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf flex-2.6.4.tar.gz

(lfs chroot) root:/sources# cd flex-2.6.4/

(lfs chroot) root:/sources/flex-2.6.4# ./configure --prefix=/usr --docdir=/usr/share/doc/flex-2.6.4
checking build system type... x86_64-pc-linux-gnu
checking host system type... x86_64-pc-linux-gnu
checking how to print strings... printf

===== TL;DR =====

config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

(lfs chroot) root:/sources/flex-2.6.4# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/flex-2.6.4# time make
Making all in src
make[1]: Entering directory '/sources/flex-2.6.4/src'
make  all-am

===== TL;DR =====

real	0m3.273s
user	0m10.860s
sys	0m0.935s

(lfs chroot) root:/sources/flex-2.6.4# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/flex-2.6.4# time make check
Making check in src
make[1]: Entering directory '/sources/flex-2.6.4/src'
make[1]: Nothing to be done for 'check'.

===== TL;DR =====

============================================================================
Testsuite summary for the fast lexical analyser generator 2.6.4
============================================================================
# TOTAL: 114
# PASS:  114
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================
make[3]: Leaving directory '/sources/flex-2.6.4/tests'
make[2]: Leaving directory '/sources/flex-2.6.4/tests'
make[1]: Leaving directory '/sources/flex-2.6.4/tests'
Making check in tools
make[1]: Entering directory '/sources/flex-2.6.4/tools'
make[1]: Nothing to be done for 'check'.
make[1]: Leaving directory '/sources/flex-2.6.4/tools'
make[1]: Entering directory '/sources/flex-2.6.4'
make[1]: Nothing to be done for 'check-am'.
make[1]: Leaving directory '/sources/flex-2.6.4'

real	0m8.171s
user	0m38.032s
sys	0m3.412s

(lfs chroot) root:/sources/flex-2.6.4# 

```

<br>
### INPUT

```
time make install
ln -sv flex /usr/bin/lex
cd ../
rm -rf flex-2.6.4/

```

### OUTPUT
```
(lfs chroot) root:/sources/flex-2.6.4# time make install
Making install in src
make[1]: Entering directory '/sources/flex-2.6.4/src'
make[2]: Entering directory '/sources/flex-2.6.4/src'

===== TL;DR =====

real	0m0.518s
user	0m0.398s
sys	0m0.119s

(lfs chroot) root:/sources/flex-2.6.4# ln -sv flex /usr/bin/lex
'/usr/bin/lex' -> 'flex'

(lfs chroot) root:/sources/flex-2.6.4# cd ../

(lfs chroot) root:/sources# rm -rf flex-2.6.4/

(lfs chroot) root:/sources#

```

<br>
# Binutils-2.35 

### INPUT
```
tar xf binutils-2.35.tar.xz
cd binutils-2.35/
expect -c "spawn ls"

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf binutils-2.35.tar.xz

(lfs chroot) root:/sources# cd binutils-2.35/

(lfs chroot) root:/sources/binutils-2.35# expect -c "spawn ls"
spawn ls

(lfs chroot) root:/sources/binutils-2.35# 

```

<br>
### INPUT
```
sed -i '/@\tincremental_copy/d' gold/testsuite/Makefile.in
mkdir -v build
cd       build
../configure --prefix=/usr       \
             --enable-gold       \
             --enable-ld=default \
             --enable-plugins    \
             --enable-shared     \
             --disable-werror    \
             --enable-64-bit-bfd \
             --with-system-zlib

```

### OUTPUT
```
(lfs chroot) root:/sources/binutils-2.35# sed -i '/@\tincremental_copy/d' gold/testsuite/Makefile.in

(lfs chroot) root:/sources/binutils-2.35# mkdir -v build
mkdir: created directory 'build'

(lfs chroot) root:/sources/binutils-2.35# cd       build

(lfs chroot) root:/sources/binutils-2.35/build# ../configure --prefix=/usr       \
>              --enable-gold       \
>              --enable-ld=default \
>              --enable-plugins    \
>              --enable-shared     \

===== TL;DR =====

checking whether to enable maintainer-specific portions of Makefiles... no
configure: creating ./config.status
config.status: creating Makefile

(lfs chroot) root:/sources/binutils-2.35/build# 

```

<br>
### INPUT
```
time make tooldir=/usr

```

### OUTPUT
```

(lfs chroot) root:/sources/binutils-2.35/build# time make tooldir=/usr
make[1]: Entering directory '/sources/binutils-2.35/build'
make[1]: Nothing to be done for 'all-target'.
mkdir -p -- ./libiberty

===== TL;DR =====

real	2m0.515s
user	8m46.501s
sys	0m38.007s
(lfs chroot) root:/sources/binutils-2.35/build# 

```

<br>
### INPUT
```
time make -k check

```

### OUTPUT
```
(lfs chroot) root:/sources/binutils-2.35/build# time make -k check
make[1]: Entering directory '/sources/binutils-2.35/build'
make[2]: Entering directory '/sources/binutils-2.35/build/etc'
make[2]: Nothing to be done for 'check'.

===== TL;DR =====

		=== binutils Summary ===

# of expected passes		274
# of unsupported tests		2

===== TL;DR =====

============================================================================
Testsuite summary for gold 0.1
============================================================================
# TOTAL: 277
# PASS:  277
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for gold 0.1
============================================================================
# TOTAL: 4
# PASS:  4
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

		=== gas Summary ===

# of expected passes		1445
/sources/binutils-2.35/build/gas/as-new 2.35

===== TL;DR =====

		=== ld Summary ===

# of expected passes		2565
# of expected failures		57
# of untested testcases		1
# of unsupported tests		23
./ld-new 2.35

===== TL;DR =====

real	4m45.134s
user	5m26.275s
sys	1m24.328s

(lfs chroot) root:/sources/binutils-2.35/build# 

```

<br>
### INPUT
```
make tooldir=/usr install
cd ../../
rm -rf binutils-2.35/

```

### OUTPUT
```
(lfs chroot) root:/sources/binutils-2.35/build# make tooldir=/usr install
make[1]: Entering directory '/sources/binutils-2.35/build'
/bin/sh ../mkinstalldirs /usr /usr
make[1]: Nothing to be done for 'install-target'.

===== TL;DR =====

----------------------------------------------------------------------
Libraries have been installed in:
   /usr/lib

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the `-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the `LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the `LD_RUN_PATH' environment variable
     during linking
   - use the `-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to `/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------

===== TL;DR =====

make[3]: Leaving directory '/sources/binutils-2.35/build/ld'
make[2]: Leaving directory '/sources/binutils-2.35/build/ld'
make[1]: Leaving directory '/sources/binutils-2.35/build'

(lfs chroot) root:/sources/binutils-2.35/build# cd ../../

(lfs chroot) root:/sources# rm -rf binutils-2.35/

(lfs chroot) root:/sources#

```

<br>
# GMP-6.2.0

### INPUT
```
tar xf gmp-6.2.0.tar.xz
cd gmp-6.2.0/
./configure --prefix=/usr    \
            --enable-cxx     \
            --disable-static \
            --docdir=/usr/share/doc/gmp-6.2.0

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf gmp-6.2.0.tar.xz

(lfs chroot) root:/sources# cd gmp-6.2.0/

(lfs chroot) root:/sources/gmp-6.2.0# ./configure --prefix=/usr    \
>             --enable-cxx     \
>             --disable-static \
>             --docdir=/usr/share/doc/gmp-6.2.0

===== TL;DR =====

configure: summary of build options:

  Version:           GNU MP 6.2.0
  Host type:         kabylake-pc-linux-gnu
  ABI:               64
  Install prefix:    /usr
  Compiler:          gcc
  Static libraries:  no
  Shared libraries:  yes

(lfs chroot) root:/sources/gmp-6.2.0# 

```

<br>
### INPUT
```
time make
time make html

```

### OUTPUT
```
(lfs chroot) root:/sources/gmp-6.2.0# time make
gcc `test -f 'gen-fac.c' || echo './'`gen-fac.c -o gen-fac
gcc `test -f 'gen-fib.c' || echo './'`gen-fib.c -o gen-fib
gcc `test -f 'gen-bases.c' || echo './'`gen-bases.c -o gen-bases -lm

===== TL;DR =====

real	0m11.202s
user	0m41.589s
sys	0m6.513s

(lfs chroot) root:/sources/gmp-6.2.0# time make html
Making html in tests
make[1]: Entering directory '/sources/gmp-6.2.0/tests'
Making html in .
make[2]: Entering directory '/sources/gmp-6.2.0/tests'

===== TL;DR =====

real	0m2.189s
user	0m1.896s
sys	0m0.037s

(lfs chroot) root:/sources/gmp-6.2.0# 

```

<br>
### INPUT
```
make check 2>&1 | tee gmp-check-log

```

### OUTPUT
```
(lfs chroot) root:/sources/gmp-6.2.0# make check 2>&1 | tee gmp-check-log
make  check-recursive
make[1]: Entering directory '/sources/gmp-6.2.0'
Making check in tests
make[2]: Entering directory '/sources/gmp-6.2.0/tests'
Making check in .

===== TL;DR =====

============================================================================
Testsuite summary for GNU MP 6.2.0
============================================================================
# TOTAL: 8
# PASS:  8
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU MP 6.2.0
============================================================================
# TOTAL: 50
# PASS:  50
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU MP 6.2.0
============================================================================
# TOTAL: 64
# PASS:  64
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU MP 6.2.0
============================================================================
# TOTAL: 15
# PASS:  15
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU MP 6.2.0
============================================================================
# TOTAL: 28
# PASS:  28
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU MP 6.2.0
============================================================================
# TOTAL: 7
# PASS:  7
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU MP 6.2.0
============================================================================
# TOTAL: 3
# PASS:  3
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU MP 6.2.0
============================================================================
# TOTAL: 22
# PASS:  22
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

make[2]: Nothing to be done for 'check-am'.
make[2]: Leaving directory '/sources/gmp-6.2.0'
make[1]: Leaving directory '/sources/gmp-6.2.0'

(lfs chroot) root:/sources/gmp-6.2.0# 

```

<br>
### INPUT
```
awk '/# PASS:/{total+=$3} ; END{print total}' gmp-check-log

```

### OUTPUT
```
(lfs chroot) root:/sources/gmp-6.2.0# awk '/# PASS:/{total+=$3} ; END{print total}' gmp-check-log
197

(lfs chroot) root:/sources/gmp-6.2.0# 

```

<br>
### INPUT
```
make install
make install-html
cd ../
rm -rf gmp-6.2.0/

```

### OUTPUT
```
(lfs chroot) root:/sources/gmp-6.2.0# make install
make  install-recursive
make[1]: Entering directory '/sources/gmp-6.2.0'
Making install in tests

===== TL;DR =====

+-------------------------------------------------------------+
| CAUTION:                                                    |
|                                                             |
| If you have not already run "make check", then we strongly  |
| recommend you do so.                                        |
|                                                             |
| GMP has been carefully tested by its authors, but compilers |
| are all too often released with serious bugs.  GMP tends to |
| explore interesting corners in compilers and has hit bugs   |
| on quite a few occasions.                                   |
|                                                             |
+-------------------------------------------------------------+

===== TL;DR =====

----------------------------------------------------------------------
Libraries have been installed in:
   /usr/lib

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the '-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the 'LD_RUN_PATH' environment variable
     during linking
   - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to '/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------

make[3]: Leaving directory '/sources/gmp-6.2.0'
make[2]: Leaving directory '/sources/gmp-6.2.0'
make[1]: Leaving directory '/sources/gmp-6.2.0'

(lfs chroot) root:/sources/gmp-6.2.0# make install-html
Making install-html in tests
make[1]: Entering directory '/sources/gmp-6.2.0/tests'
Making install-html in .
make[2]: Entering directory '/sources/gmp-6.2.0/tests'

===== TL;DR =====

make[1]: Entering directory '/sources/gmp-6.2.0'
make[1]: Nothing to be done for 'install-html-am'.
make[1]: Leaving directory '/sources/gmp-6.2.0'

(lfs chroot) root:/sources/gmp-6.2.0# cd ../

(lfs chroot) root:/sources# rm -rf gmp-6.2.0/

(lfs chroot) root:/sources#

```

<br>
# MPFR-4.1.0

### INPUT
```
tar xf mpfr-4.1.0.tar.xz
cd mpfr-4.1.0/
./configure --prefix=/usr        \
            --disable-static     \
            --enable-thread-safe \
            --docdir=/usr/share/doc/mpfr-4.1.0

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf mpfr-4.1.0.tar.xz

(lfs chroot) root:/sources# cd mpfr-4.1.0/

(lfs chroot) root:/sources/mpfr-4.1.0# ./configure --prefix=/usr        \
>             --disable-static     \
>             --enable-thread-safe \
>             --docdir=/usr/share/doc/mpfr-4.1.0

===== TL;DR =====

config.status: creating tools/bench/Makefile
config.status: executing depfiles commands
config.status: executing libtool commands

(lfs chroot) root:/sources/mpfr-4.1.0# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/mpfr-4.1.0# time make
Making all in doc
make[1]: Entering directory '/sources/mpfr-4.1.0/doc'
make[1]: Nothing to be done for 'all'.
make[1]: Leaving directory '/sources/mpfr-4.1.0/doc'

===== TL;DR =====

make[1]: Entering directory '/sources/mpfr-4.1.0'
make[1]: Nothing to be done for 'all-am'.
make[1]: Leaving directory '/sources/mpfr-4.1.0'

real	0m6.602s
user	0m28.749s
sys	0m3.936s

(lfs chroot) root:/sources/mpfr-4.1.0# 

```

<br>
### INPUT
```
time make html

```

### OUTPUT
```
(lfs chroot) root:/sources/mpfr-4.1.0# time make html
Making html in doc
make[1]: Entering directory '/sources/mpfr-4.1.0/doc'
rm -rf mpfr.htp

===== TL;DR =====

real	0m1.730s
user	0m1.170s
sys	0m0.055s

(lfs chroot) root:/sources/mpfr-4.1.0# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```

(lfs chroot) root:/sources/mpfr-4.1.0# 
(lfs chroot) root:/sources/mpfr-4.1.0# time make check
Making check in doc
make[1]: Entering directory '/sources/mpfr-4.1.0/doc'
make[1]: Nothing to be done for 'check'.
make[1]: Leaving directory '/sources/mpfr-4.1.0/doc'
Making check in src
make[1]: Entering directory '/sources/mpfr-4.1.0/src'
make  check-am
make[2]: Entering directory '/sources/mpfr-4.1.0/src'
make[2]: Nothing to be done for 'check-am'.
make[2]: Leaving directory '/sources/mpfr-4.1.0/src'
make[1]: Leaving directory '/sources/mpfr-4.1.0/src'
Making check in tests
make[1]: Entering directory '/sources/mpfr-4.1.0/tests'
make  tversion tabort_prec_max tassert tabort_defalloc1 tabort_defalloc2 talloc tinternals tinits tisqrt tsgn tcheck tisnan texceptions tset_exp tset mpf_compat mpfr_compat reuse tabs tacos tacosh tadd tadd1sp tadd_d tadd_ui tagm tai talloc-cache tasin tasinh tatan tatanh taway tbeta tbuildopt tcan_round tcbrt tcmp tcmp2 tcmp_d tcmp_ld tcmp_ui tcmpabs tcomparisons tconst_catalan tconst_euler tconst_log2 tconst_pi tcopysign tcos tcosh tcot tcoth tcsc tcsch td_div td_sub tdigamma tdim tdiv tdiv_d tdiv_ui tdot teint teq terandom terandom_chisq terf texp texp10 texp2 texpm1 tfactorial tfits tfma tfmma tfmod tfms tfpif tfprintf tfrac tfrexp tgamma tgamma_inc tget_d tget_d_2exp tget_f tget_flt tget_ld_2exp tget_q tget_set_d64 tget_set_d128 tget_sj tget_str tget_z tgmpop tgrandom thyperbolic thypot tinp_str tj0 tj1 tjn tl2b tlgamma tli2 tlngamma tlog tlog10 tlog1p tlog2 tlog_ui tmin_prec tminmax tmodf tmul tmul_2exp tmul_d tmul_ui tnext tnrandom tnrandom_chisq tout_str toutimpl tpow tpow3 tpow_all tpow_z tprec_round tprintf trandom trandom_deviate trec_sqrt tremquo trint trndna troot trootn_ui tsec tsech tset_d tset_f tset_float128 tset_ld tset_q tset_si tset_sj tset_str tset_z tset_z_exp tsi_op tsin tsin_cos tsinh tsinh_cosh tsprintf tsqr tsqrt tsqrt_ui tstckintc tstdint tstrtofr tsub tsub1sp tsub_d tsub_ui tsubnormal tsum tswap ttan ttanh ttotal_order ttrunc tui_div tui_pow tui_sub turandom tvalist ty0 ty1 tyn tzeta tzeta_ui libfrtests.la
make[2]: Entering directory '/sources/mpfr-4.1.0/tests'
depbase=`echo tversion.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tversion.o -MD -MP -MF $depbase.Tpo -c -o tversion.o tversion.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo memory.lo | sed 's|[^/]*$|.deps/&|;s|\.lo$||'`;\
/bin/sh ../libtool  --tag=CC   --mode=compile gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT memory.lo -MD -MP -MF $depbase.Tpo -c -o memory.lo memory.c &&\
mv -f $depbase.Tpo $depbase.Plo
depbase=`echo rnd_mode.lo | sed 's|[^/]*$|.deps/&|;s|\.lo$||'`;\
/bin/sh ../libtool  --tag=CC   --mode=compile gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT rnd_mode.lo -MD -MP -MF $depbase.Tpo -c -o rnd_mode.lo rnd_mode.c &&\
mv -f $depbase.Tpo $depbase.Plo
depbase=`echo tests.lo | sed 's|[^/]*$|.deps/&|;s|\.lo$||'`;\
/bin/sh ../libtool  --tag=CC   --mode=compile gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tests.lo -MD -MP -MF $depbase.Tpo -c -o tests.lo tests.c &&\
mv -f $depbase.Tpo $depbase.Plo
depbase=`echo cmp_str.lo | sed 's|[^/]*$|.deps/&|;s|\.lo$||'`;\
/bin/sh ../libtool  --tag=CC   --mode=compile gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT cmp_str.lo -MD -MP -MF $depbase.Tpo -c -o cmp_str.lo cmp_str.c &&\
mv -f $depbase.Tpo $depbase.Plo
depbase=`echo random2.lo | sed 's|[^/]*$|.deps/&|;s|\.lo$||'`;\
/bin/sh ../libtool  --tag=CC   --mode=compile gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT random2.lo -MD -MP -MF $depbase.Tpo -c -o random2.lo random2.c &&\
mv -f $depbase.Tpo $depbase.Plo
depbase=`echo tabort_prec_max.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tabort_prec_max.o -MD -MP -MF $depbase.Tpo -c -o tabort_prec_max.o tabort_prec_max.c &&\
mv -f $depbase.Tpo $depbase.Po
libtool: compile:  gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I. -DSRCDIR=\".\" -I../src -I../src -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT memory.lo -MD -MP -MF .deps/memory.Tpo -c memory.c  -fPIC -DPIC -o .libs/memory.o
libtool: compile:  gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I. -DSRCDIR=\".\" -I../src -I../src -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tests.lo -MD -MP -MF .deps/tests.Tpo -c tests.c  -fPIC -DPIC -o .libs/tests.o
libtool: compile:  gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I. -DSRCDIR=\".\" -I../src -I../src -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT rnd_mode.lo -MD -MP -MF .deps/rnd_mode.Tpo -c rnd_mode.c  -fPIC -DPIC -o .libs/rnd_mode.o
libtool: compile:  gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I. -DSRCDIR=\".\" -I../src -I../src -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT cmp_str.lo -MD -MP -MF .deps/cmp_str.Tpo -c cmp_str.c  -fPIC -DPIC -o .libs/cmp_str.o
depbase=`echo tassert.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tassert.o -MD -MP -MF $depbase.Tpo -c -o tassert.o tassert.c &&\
mv -f $depbase.Tpo $depbase.Po
libtool: compile:  gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I. -DSRCDIR=\".\" -I../src -I../src -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT random2.lo -MD -MP -MF .deps/random2.Tpo -c random2.c  -fPIC -DPIC -o .libs/random2.o
depbase=`echo tabort_defalloc1.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tabort_defalloc1.o -MD -MP -MF $depbase.Tpo -c -o tabort_defalloc1.o tabort_defalloc1.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tabort_defalloc2.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tabort_defalloc2.o -MD -MP -MF $depbase.Tpo -c -o tabort_defalloc2.o tabort_defalloc2.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo talloc.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT talloc.o -MD -MP -MF $depbase.Tpo -c -o talloc.o talloc.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tinternals.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tinternals.o -MD -MP -MF $depbase.Tpo -c -o tinternals.o tinternals.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tinits.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tinits.o -MD -MP -MF $depbase.Tpo -c -o tinits.o tinits.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tisqrt.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tisqrt.o -MD -MP -MF $depbase.Tpo -c -o tisqrt.o tisqrt.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsgn.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsgn.o -MD -MP -MF $depbase.Tpo -c -o tsgn.o tsgn.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcheck.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcheck.o -MD -MP -MF $depbase.Tpo -c -o tcheck.o tcheck.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tisnan.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tisnan.o -MD -MP -MF $depbase.Tpo -c -o tisnan.o tisnan.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo texceptions.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT texceptions.o -MD -MP -MF $depbase.Tpo -c -o texceptions.o texceptions.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_exp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_exp.o -MD -MP -MF $depbase.Tpo -c -o tset_exp.o tset_exp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset.o -MD -MP -MF $depbase.Tpo -c -o tset.o tset.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo mpf_compat.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT mpf_compat.o -MD -MP -MF $depbase.Tpo -c -o mpf_compat.o mpf_compat.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo mpfr_compat.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT mpfr_compat.o -MD -MP -MF $depbase.Tpo -c -o mpfr_compat.o mpfr_compat.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo reuse.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT reuse.o -MD -MP -MF $depbase.Tpo -c -o reuse.o reuse.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tabs.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tabs.o -MD -MP -MF $depbase.Tpo -c -o tabs.o tabs.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tacos.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tacos.o -MD -MP -MF $depbase.Tpo -c -o tacos.o tacos.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tacosh.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tacosh.o -MD -MP -MF $depbase.Tpo -c -o tacosh.o tacosh.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tadd.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tadd.o -MD -MP -MF $depbase.Tpo -c -o tadd.o tadd.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tadd1sp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tadd1sp.o -MD -MP -MF $depbase.Tpo -c -o tadd1sp.o tadd1sp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tadd_d.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tadd_d.o -MD -MP -MF $depbase.Tpo -c -o tadd_d.o tadd_d.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tadd_ui.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tadd_ui.o -MD -MP -MF $depbase.Tpo -c -o tadd_ui.o tadd_ui.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tagm.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tagm.o -MD -MP -MF $depbase.Tpo -c -o tagm.o tagm.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tai.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tai.o -MD -MP -MF $depbase.Tpo -c -o tai.o tai.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo talloc-cache.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT talloc-cache.o -MD -MP -MF $depbase.Tpo -c -o talloc-cache.o talloc-cache.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tasin.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tasin.o -MD -MP -MF $depbase.Tpo -c -o tasin.o tasin.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tasinh.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tasinh.o -MD -MP -MF $depbase.Tpo -c -o tasinh.o tasinh.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tatan.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tatan.o -MD -MP -MF $depbase.Tpo -c -o tatan.o tatan.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tatanh.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tatanh.o -MD -MP -MF $depbase.Tpo -c -o tatanh.o tatanh.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo taway.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT taway.o -MD -MP -MF $depbase.Tpo -c -o taway.o taway.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tbeta.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tbeta.o -MD -MP -MF $depbase.Tpo -c -o tbeta.o tbeta.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tbuildopt.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tbuildopt.o -MD -MP -MF $depbase.Tpo -c -o tbuildopt.o tbuildopt.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcan_round.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcan_round.o -MD -MP -MF $depbase.Tpo -c -o tcan_round.o tcan_round.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcbrt.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcbrt.o -MD -MP -MF $depbase.Tpo -c -o tcbrt.o tcbrt.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcmp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcmp.o -MD -MP -MF $depbase.Tpo -c -o tcmp.o tcmp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcmp2.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcmp2.o -MD -MP -MF $depbase.Tpo -c -o tcmp2.o tcmp2.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcmp_d.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcmp_d.o -MD -MP -MF $depbase.Tpo -c -o tcmp_d.o tcmp_d.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcmp_ld.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcmp_ld.o -MD -MP -MF $depbase.Tpo -c -o tcmp_ld.o tcmp_ld.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcmp_ui.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcmp_ui.o -MD -MP -MF $depbase.Tpo -c -o tcmp_ui.o tcmp_ui.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcmpabs.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcmpabs.o -MD -MP -MF $depbase.Tpo -c -o tcmpabs.o tcmpabs.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcomparisons.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcomparisons.o -MD -MP -MF $depbase.Tpo -c -o tcomparisons.o tcomparisons.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tconst_catalan.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tconst_catalan.o -MD -MP -MF $depbase.Tpo -c -o tconst_catalan.o tconst_catalan.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tconst_euler.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tconst_euler.o -MD -MP -MF $depbase.Tpo -c -o tconst_euler.o tconst_euler.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tconst_log2.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tconst_log2.o -MD -MP -MF $depbase.Tpo -c -o tconst_log2.o tconst_log2.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tconst_pi.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tconst_pi.o -MD -MP -MF $depbase.Tpo -c -o tconst_pi.o tconst_pi.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcopysign.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcopysign.o -MD -MP -MF $depbase.Tpo -c -o tcopysign.o tcopysign.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcos.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcos.o -MD -MP -MF $depbase.Tpo -c -o tcos.o tcos.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcosh.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcosh.o -MD -MP -MF $depbase.Tpo -c -o tcosh.o tcosh.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcot.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcot.o -MD -MP -MF $depbase.Tpo -c -o tcot.o tcot.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcoth.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcoth.o -MD -MP -MF $depbase.Tpo -c -o tcoth.o tcoth.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcsc.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcsc.o -MD -MP -MF $depbase.Tpo -c -o tcsc.o tcsc.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tcsch.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tcsch.o -MD -MP -MF $depbase.Tpo -c -o tcsch.o tcsch.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo td_div.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT td_div.o -MD -MP -MF $depbase.Tpo -c -o td_div.o td_div.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo td_sub.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT td_sub.o -MD -MP -MF $depbase.Tpo -c -o td_sub.o td_sub.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tdigamma.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tdigamma.o -MD -MP -MF $depbase.Tpo -c -o tdigamma.o tdigamma.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tdim.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tdim.o -MD -MP -MF $depbase.Tpo -c -o tdim.o tdim.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tdiv.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tdiv.o -MD -MP -MF $depbase.Tpo -c -o tdiv.o tdiv.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tdiv_d.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tdiv_d.o -MD -MP -MF $depbase.Tpo -c -o tdiv_d.o tdiv_d.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tdiv_ui.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tdiv_ui.o -MD -MP -MF $depbase.Tpo -c -o tdiv_ui.o tdiv_ui.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tdot.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tdot.o -MD -MP -MF $depbase.Tpo -c -o tdot.o tdot.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo teint.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT teint.o -MD -MP -MF $depbase.Tpo -c -o teint.o teint.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo teq.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT teq.o -MD -MP -MF $depbase.Tpo -c -o teq.o teq.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo terandom.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT terandom.o -MD -MP -MF $depbase.Tpo -c -o terandom.o terandom.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo terandom_chisq.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT terandom_chisq.o -MD -MP -MF $depbase.Tpo -c -o terandom_chisq.o terandom_chisq.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo terf.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT terf.o -MD -MP -MF $depbase.Tpo -c -o terf.o terf.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo texp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT texp.o -MD -MP -MF $depbase.Tpo -c -o texp.o texp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo texp10.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT texp10.o -MD -MP -MF $depbase.Tpo -c -o texp10.o texp10.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo texp2.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT texp2.o -MD -MP -MF $depbase.Tpo -c -o texp2.o texp2.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo texpm1.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT texpm1.o -MD -MP -MF $depbase.Tpo -c -o texpm1.o texpm1.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfactorial.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfactorial.o -MD -MP -MF $depbase.Tpo -c -o tfactorial.o tfactorial.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfits.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfits.o -MD -MP -MF $depbase.Tpo -c -o tfits.o tfits.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfma.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfma.o -MD -MP -MF $depbase.Tpo -c -o tfma.o tfma.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfmma.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfmma.o -MD -MP -MF $depbase.Tpo -c -o tfmma.o tfmma.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfmod.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfmod.o -MD -MP -MF $depbase.Tpo -c -o tfmod.o tfmod.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfms.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfms.o -MD -MP -MF $depbase.Tpo -c -o tfms.o tfms.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfpif.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfpif.o -MD -MP -MF $depbase.Tpo -c -o tfpif.o tfpif.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfprintf.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfprintf.o -MD -MP -MF $depbase.Tpo -c -o tfprintf.o tfprintf.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfrac.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfrac.o -MD -MP -MF $depbase.Tpo -c -o tfrac.o tfrac.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tfrexp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tfrexp.o -MD -MP -MF $depbase.Tpo -c -o tfrexp.o tfrexp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tgamma.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tgamma.o -MD -MP -MF $depbase.Tpo -c -o tgamma.o tgamma.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tgamma_inc.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tgamma_inc.o -MD -MP -MF $depbase.Tpo -c -o tgamma_inc.o tgamma_inc.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_d.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_d.o -MD -MP -MF $depbase.Tpo -c -o tget_d.o tget_d.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_d_2exp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_d_2exp.o -MD -MP -MF $depbase.Tpo -c -o tget_d_2exp.o tget_d_2exp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_f.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_f.o -MD -MP -MF $depbase.Tpo -c -o tget_f.o tget_f.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_flt.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_flt.o -MD -MP -MF $depbase.Tpo -c -o tget_flt.o tget_flt.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_ld_2exp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_ld_2exp.o -MD -MP -MF $depbase.Tpo -c -o tget_ld_2exp.o tget_ld_2exp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_q.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_q.o -MD -MP -MF $depbase.Tpo -c -o tget_q.o tget_q.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_set_d64.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_set_d64.o -MD -MP -MF $depbase.Tpo -c -o tget_set_d64.o tget_set_d64.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_set_d128.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_set_d128.o -MD -MP -MF $depbase.Tpo -c -o tget_set_d128.o tget_set_d128.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_sj.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_sj.o -MD -MP -MF $depbase.Tpo -c -o tget_sj.o tget_sj.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_str.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_str.o -MD -MP -MF $depbase.Tpo -c -o tget_str.o tget_str.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tget_z.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tget_z.o -MD -MP -MF $depbase.Tpo -c -o tget_z.o tget_z.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tgmpop.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tgmpop.o -MD -MP -MF $depbase.Tpo -c -o tgmpop.o tgmpop.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tgrandom.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tgrandom.o -MD -MP -MF $depbase.Tpo -c -o tgrandom.o tgrandom.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo thyperbolic.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT thyperbolic.o -MD -MP -MF $depbase.Tpo -c -o thyperbolic.o thyperbolic.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo thypot.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT thypot.o -MD -MP -MF $depbase.Tpo -c -o thypot.o thypot.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tinp_str.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tinp_str.o -MD -MP -MF $depbase.Tpo -c -o tinp_str.o tinp_str.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tj0.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tj0.o -MD -MP -MF $depbase.Tpo -c -o tj0.o tj0.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tj1.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tj1.o -MD -MP -MF $depbase.Tpo -c -o tj1.o tj1.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tjn.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tjn.o -MD -MP -MF $depbase.Tpo -c -o tjn.o tjn.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tl2b.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tl2b.o -MD -MP -MF $depbase.Tpo -c -o tl2b.o tl2b.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tlgamma.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tlgamma.o -MD -MP -MF $depbase.Tpo -c -o tlgamma.o tlgamma.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tli2.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tli2.o -MD -MP -MF $depbase.Tpo -c -o tli2.o tli2.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tlngamma.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tlngamma.o -MD -MP -MF $depbase.Tpo -c -o tlngamma.o tlngamma.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tlog.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tlog.o -MD -MP -MF $depbase.Tpo -c -o tlog.o tlog.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tlog10.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tlog10.o -MD -MP -MF $depbase.Tpo -c -o tlog10.o tlog10.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tlog1p.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tlog1p.o -MD -MP -MF $depbase.Tpo -c -o tlog1p.o tlog1p.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tlog2.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tlog2.o -MD -MP -MF $depbase.Tpo -c -o tlog2.o tlog2.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tlog_ui.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tlog_ui.o -MD -MP -MF $depbase.Tpo -c -o tlog_ui.o tlog_ui.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tmin_prec.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tmin_prec.o -MD -MP -MF $depbase.Tpo -c -o tmin_prec.o tmin_prec.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tminmax.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tminmax.o -MD -MP -MF $depbase.Tpo -c -o tminmax.o tminmax.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tmodf.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tmodf.o -MD -MP -MF $depbase.Tpo -c -o tmodf.o tmodf.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tmul.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tmul.o -MD -MP -MF $depbase.Tpo -c -o tmul.o tmul.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tmul_2exp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tmul_2exp.o -MD -MP -MF $depbase.Tpo -c -o tmul_2exp.o tmul_2exp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tmul_d.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tmul_d.o -MD -MP -MF $depbase.Tpo -c -o tmul_d.o tmul_d.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tmul_ui.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tmul_ui.o -MD -MP -MF $depbase.Tpo -c -o tmul_ui.o tmul_ui.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tnext.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tnext.o -MD -MP -MF $depbase.Tpo -c -o tnext.o tnext.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tnrandom.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tnrandom.o -MD -MP -MF $depbase.Tpo -c -o tnrandom.o tnrandom.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tnrandom_chisq.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tnrandom_chisq.o -MD -MP -MF $depbase.Tpo -c -o tnrandom_chisq.o tnrandom_chisq.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tout_str.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tout_str.o -MD -MP -MF $depbase.Tpo -c -o tout_str.o tout_str.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo toutimpl.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT toutimpl.o -MD -MP -MF $depbase.Tpo -c -o toutimpl.o toutimpl.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tpow.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tpow.o -MD -MP -MF $depbase.Tpo -c -o tpow.o tpow.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tpow3.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tpow3.o -MD -MP -MF $depbase.Tpo -c -o tpow3.o tpow3.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tpow_all.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tpow_all.o -MD -MP -MF $depbase.Tpo -c -o tpow_all.o tpow_all.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tpow_z.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tpow_z.o -MD -MP -MF $depbase.Tpo -c -o tpow_z.o tpow_z.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tprec_round.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tprec_round.o -MD -MP -MF $depbase.Tpo -c -o tprec_round.o tprec_round.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tprintf.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tprintf.o -MD -MP -MF $depbase.Tpo -c -o tprintf.o tprintf.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo trandom.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT trandom.o -MD -MP -MF $depbase.Tpo -c -o trandom.o trandom.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo trandom_deviate.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT trandom_deviate.o -MD -MP -MF $depbase.Tpo -c -o trandom_deviate.o trandom_deviate.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo trec_sqrt.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT trec_sqrt.o -MD -MP -MF $depbase.Tpo -c -o trec_sqrt.o trec_sqrt.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tremquo.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tremquo.o -MD -MP -MF $depbase.Tpo -c -o tremquo.o tremquo.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo trint.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT trint.o -MD -MP -MF $depbase.Tpo -c -o trint.o trint.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo trndna.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT trndna.o -MD -MP -MF $depbase.Tpo -c -o trndna.o trndna.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo troot.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT troot.o -MD -MP -MF $depbase.Tpo -c -o troot.o troot.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo trootn_ui.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT trootn_ui.o -MD -MP -MF $depbase.Tpo -c -o trootn_ui.o trootn_ui.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsec.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsec.o -MD -MP -MF $depbase.Tpo -c -o tsec.o tsec.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsech.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsech.o -MD -MP -MF $depbase.Tpo -c -o tsech.o tsech.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_d.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_d.o -MD -MP -MF $depbase.Tpo -c -o tset_d.o tset_d.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_f.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_f.o -MD -MP -MF $depbase.Tpo -c -o tset_f.o tset_f.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_float128.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_float128.o -MD -MP -MF $depbase.Tpo -c -o tset_float128.o tset_float128.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_ld.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_ld.o -MD -MP -MF $depbase.Tpo -c -o tset_ld.o tset_ld.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_q.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_q.o -MD -MP -MF $depbase.Tpo -c -o tset_q.o tset_q.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_si.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_si.o -MD -MP -MF $depbase.Tpo -c -o tset_si.o tset_si.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_sj.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_sj.o -MD -MP -MF $depbase.Tpo -c -o tset_sj.o tset_sj.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_str.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_str.o -MD -MP -MF $depbase.Tpo -c -o tset_str.o tset_str.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_z.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_z.o -MD -MP -MF $depbase.Tpo -c -o tset_z.o tset_z.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tset_z_exp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tset_z_exp.o -MD -MP -MF $depbase.Tpo -c -o tset_z_exp.o tset_z_exp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsi_op.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsi_op.o -MD -MP -MF $depbase.Tpo -c -o tsi_op.o tsi_op.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsin.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsin.o -MD -MP -MF $depbase.Tpo -c -o tsin.o tsin.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsin_cos.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsin_cos.o -MD -MP -MF $depbase.Tpo -c -o tsin_cos.o tsin_cos.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsinh.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsinh.o -MD -MP -MF $depbase.Tpo -c -o tsinh.o tsinh.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsinh_cosh.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsinh_cosh.o -MD -MP -MF $depbase.Tpo -c -o tsinh_cosh.o tsinh_cosh.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsprintf.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsprintf.o -MD -MP -MF $depbase.Tpo -c -o tsprintf.o tsprintf.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsqr.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsqr.o -MD -MP -MF $depbase.Tpo -c -o tsqr.o tsqr.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsqrt.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsqrt.o -MD -MP -MF $depbase.Tpo -c -o tsqrt.o tsqrt.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsqrt_ui.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsqrt_ui.o -MD -MP -MF $depbase.Tpo -c -o tsqrt_ui.o tsqrt_ui.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tstckintc.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tstckintc.o -MD -MP -MF $depbase.Tpo -c -o tstckintc.o tstckintc.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tstdint.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tstdint.o -MD -MP -MF $depbase.Tpo -c -o tstdint.o tstdint.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tstrtofr.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tstrtofr.o -MD -MP -MF $depbase.Tpo -c -o tstrtofr.o tstrtofr.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsub.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsub.o -MD -MP -MF $depbase.Tpo -c -o tsub.o tsub.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsub1sp.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsub1sp.o -MD -MP -MF $depbase.Tpo -c -o tsub1sp.o tsub1sp.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsub_d.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsub_d.o -MD -MP -MF $depbase.Tpo -c -o tsub_d.o tsub_d.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsub_ui.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsub_ui.o -MD -MP -MF $depbase.Tpo -c -o tsub_ui.o tsub_ui.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsubnormal.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsubnormal.o -MD -MP -MF $depbase.Tpo -c -o tsubnormal.o tsubnormal.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tsum.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tsum.o -MD -MP -MF $depbase.Tpo -c -o tsum.o tsum.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tswap.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tswap.o -MD -MP -MF $depbase.Tpo -c -o tswap.o tswap.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo ttan.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT ttan.o -MD -MP -MF $depbase.Tpo -c -o ttan.o ttan.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo ttanh.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT ttanh.o -MD -MP -MF $depbase.Tpo -c -o ttanh.o ttanh.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo ttotal_order.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT ttotal_order.o -MD -MP -MF $depbase.Tpo -c -o ttotal_order.o ttotal_order.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo ttrunc.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT ttrunc.o -MD -MP -MF $depbase.Tpo -c -o ttrunc.o ttrunc.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tui_div.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tui_div.o -MD -MP -MF $depbase.Tpo -c -o tui_div.o tui_div.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tui_pow.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tui_pow.o -MD -MP -MF $depbase.Tpo -c -o tui_pow.o tui_pow.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tui_sub.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tui_sub.o -MD -MP -MF $depbase.Tpo -c -o tui_sub.o tui_sub.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo turandom.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT turandom.o -MD -MP -MF $depbase.Tpo -c -o turandom.o turandom.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tvalist.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tvalist.o -MD -MP -MF $depbase.Tpo -c -o tvalist.o tvalist.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo ty0.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT ty0.o -MD -MP -MF $depbase.Tpo -c -o ty0.o ty0.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo ty1.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT ty1.o -MD -MP -MF $depbase.Tpo -c -o ty1.o ty1.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tyn.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tyn.o -MD -MP -MF $depbase.Tpo -c -o tyn.o tyn.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tzeta.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tzeta.o -MD -MP -MF $depbase.Tpo -c -o tzeta.o tzeta.c &&\
mv -f $depbase.Tpo $depbase.Po
depbase=`echo tzeta_ui.o | sed 's|[^/]*$|.deps/&|;s|\.o$||'`;\
gcc -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DLT_OBJDIR=\".libs/\" -DHAVE_LITTLE_ENDIAN=1 -DHAVE_CLOCK_GETTIME=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_LOCALE_H=1 -DHAVE_WCHAR_H=1 -DHAVE_STDARG=1 -DHAVE_SYS_TIME_H=1 -DHAVE_STRUCT_LCONV_DECIMAL_POINT=1 -DHAVE_STRUCT_LCONV_THOUSANDS_SEP=1 -DHAVE_ALLOCA_H=1 -DHAVE_ALLOCA=1 -DHAVE_UINTPTR_T=1 -DHAVE_VA_COPY=1 -DHAVE_SETLOCALE=1 -DHAVE_GETTIMEOFDAY=1 -DHAVE_SIGNAL=1 -DHAVE_SIGACTION=1 -DHAVE_LONG_LONG=1 -DHAVE_INTMAX_T=1 -DMPFR_HAVE_INTMAX_MAX=1 -DMPFR_HAVE_NORETURN=1 -DMPFR_HAVE_BUILTIN_UNREACHABLE=1 -DMPFR_HAVE_CONSTRUCTOR_ATTR=1 -DMPFR_HAVE_FESETROUND=1 -DHAVE_SUBNORM_DBL=1 -DHAVE_SUBNORM_FLT=1 -DHAVE_SIGNEDZ=1 -DHAVE_ROUND=1 -DHAVE_TRUNC=1 -DHAVE_FLOOR=1 -DHAVE_CEIL=1 -DHAVE_NEARBYINT=1 -DHAVE_MULX_U64=1 -DHAVE_DOUBLE_IEEE_LITTLE_ENDIAN=1 -DHAVE_LDOUBLE_IEEE_EXT_LITTLE=1 -DMPFR_USE_THREAD_SAFE=1 -DMPFR_USE_C11_THREAD_SAFE=1 -DMPFR_WANT_FLOAT128=1 -DMPFR_USE_STATIC_ASSERT=1 -DHAVE_ATTRIBUTE_MODE=1 -DPRINTF_L=1 -DPRINTF_T=1 -DPRINTF_GROUPFLAG=1 -DHAVE___GMPN_SBPI1_DIVAPPR_Q=1 -DHAVE___GMPN_INVERT_LIMB=1 -DHAVE___GMPN_RSBLSH1_N=1 -DMPFR_LONG_WITHIN_LIMB=1 -DMPFR_INTMAX_WITHIN_LIMB=1 -DHAVE_GETRUSAGE=1 -I.  -DSRCDIR='"."' -I../src -I../src   -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -MT tzeta_ui.o -MD -MP -MF $depbase.Tpo -c -o tzeta_ui.o tzeta_ui.c &&\
mv -f $depbase.Tpo $depbase.Po
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o libfrtests.la  memory.lo rnd_mode.lo tests.lo cmp_str.lo random2.lo  -lgmp 
libtool: link: ar cr .libs/libfrtests.a .libs/memory.o .libs/rnd_mode.o .libs/tests.o .libs/cmp_str.o .libs/random2.o 
libtool: link: ranlib .libs/libfrtests.a
libtool: link: ( cd ".libs" && rm -f "libfrtests.la" && ln -s "../libfrtests.la" "libfrtests.la" )
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tversion tversion.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tabort_prec_max tabort_prec_max.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tassert tassert.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tabort_defalloc1 tabort_defalloc1.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tabort_defalloc2 tabort_defalloc2.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o talloc talloc.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tabort_prec_max tabort_prec_max.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tassert tassert.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tversion tversion.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tabort_defalloc1 tabort_defalloc1.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tabort_defalloc2 tabort_defalloc2.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tinternals tinternals.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tinits tinits.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tisqrt tisqrt.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsgn tsgn.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcheck tcheck.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o talloc talloc.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tinternals tinternals.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tinits tinits.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tisnan tisnan.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tisqrt tisqrt.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsgn tsgn.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o texceptions texceptions.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_exp tset_exp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset tset.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcheck tcheck.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o mpf_compat mpf_compat.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o mpfr_compat mpfr_compat.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tisnan tisnan.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o texceptions texceptions.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_exp tset_exp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o reuse reuse.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset tset.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tabs tabs.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o mpf_compat mpf_compat.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tacos tacos.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tacosh tacosh.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o mpfr_compat mpfr_compat.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tadd tadd.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tadd1sp tadd1sp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o reuse reuse.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tacosh tacosh.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tacos tacos.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tabs tabs.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tadd_d tadd_d.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tadd_ui tadd_ui.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tadd1sp tadd1sp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tadd tadd.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tagm tagm.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tai tai.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o talloc-cache talloc-cache.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tasin tasin.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tadd_ui tadd_ui.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tadd_d tadd_d.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tagm tagm.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tasinh tasinh.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tatan tatan.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tasin tasin.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tai tai.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o talloc-cache talloc-cache.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tatanh tatanh.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tbeta tbeta.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tbuildopt tbuildopt.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o taway taway.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tasinh tasinh.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tatan tatan.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tatanh tatanh.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcan_round tcan_round.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tbuildopt tbuildopt.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcbrt tcbrt.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o taway taway.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tbeta tbeta.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcmp tcmp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcmp2 tcmp2.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcmp_d tcmp_d.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcmp_ld tcmp_ld.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcan_round tcan_round.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcbrt tcbrt.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcmp tcmp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcmp_ui tcmp_ui.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcmpabs tcmpabs.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcmp2 tcmp2.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcomparisons tcomparisons.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcmp_d tcmp_d.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tconst_catalan tconst_catalan.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcmp_ld tcmp_ld.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tconst_euler tconst_euler.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tconst_log2 tconst_log2.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcmpabs tcmpabs.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcmp_ui tcmp_ui.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcomparisons tcomparisons.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tconst_pi tconst_pi.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tconst_catalan tconst_catalan.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcopysign tcopysign.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcos tcos.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tconst_euler tconst_euler.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcosh tcosh.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tconst_log2 tconst_log2.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcot tcot.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcoth tcoth.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tconst_pi tconst_pi.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcopysign tcopysign.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcos tcos.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcsc tcsc.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcosh tcosh.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tcsch tcsch.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o td_div td_div.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o td_sub td_sub.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcot tcot.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcoth tcoth.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tdigamma tdigamma.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tdim tdim.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcsc tcsc.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tdiv tdiv.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o td_sub td_sub.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tcsch tcsch.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o td_div td_div.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tdiv_d tdiv_d.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tdiv_ui tdiv_ui.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tdigamma tdigamma.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tdot tdot.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o teint teint.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tdim tdim.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o teq teq.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tdiv tdiv.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tdiv_ui tdiv_ui.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tdot tdot.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tdiv_d tdiv_d.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o terandom terandom.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o terandom_chisq terandom_chisq.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o terf terf.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o texp texp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o teint teint.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o texp10 texp10.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o teq teq.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o texp2 texp2.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o terandom_chisq terandom_chisq.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o terandom terandom.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o terf terf.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o texp texp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o texpm1 texpm1.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfactorial tfactorial.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfits tfits.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o texp10 texp10.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfma tfma.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfmma tfmma.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o texp2 texp2.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfmod tfmod.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o texpm1 texpm1.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfma tfma.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfactorial tfactorial.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfits tfits.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfms tfms.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfpif tfpif.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfmma tfmma.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfprintf tfprintf.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfrac tfrac.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tfrexp tfrexp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfmod tfmod.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tgamma tgamma.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfms tfms.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfpif tfpif.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tgamma_inc tgamma_inc.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfrac tfrac.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfrexp tfrexp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tfprintf tfprintf.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_d tget_d.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_d_2exp tget_d_2exp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_f tget_f.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_flt tget_flt.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tgamma tgamma.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_ld_2exp tget_ld_2exp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tgamma_inc tgamma_inc.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_d_2exp tget_d_2exp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_d tget_d.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_q tget_q.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_flt tget_flt.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_f tget_f.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_set_d64 tget_set_d64.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_set_d128 tget_set_d128.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_sj tget_sj.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_str tget_str.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_ld_2exp tget_ld_2exp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tget_z tget_z.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_q tget_q.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_set_d128 tget_set_d128.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_sj tget_sj.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_set_d64 tget_set_d64.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tgmpop tgmpop.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tgrandom tgrandom.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o thyperbolic thyperbolic.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o thypot thypot.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_str tget_str.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tget_z tget_z.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tinp_str tinp_str.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tj0 tj0.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tgmpop tgmpop.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tgrandom tgrandom.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o thyperbolic thyperbolic.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o thypot thypot.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tj1 tj1.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tjn tjn.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tl2b tl2b.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tlgamma tlgamma.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tinp_str tinp_str.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tli2 tli2.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tj0 tj0.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tlngamma tlngamma.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tl2b tl2b.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tjn tjn.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tj1 tj1.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tlgamma tlgamma.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tlog tlog.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tlog10 tlog10.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tli2 tli2.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tlog1p tlog1p.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tlog2 tlog2.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tlog_ui tlog_ui.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tlngamma tlngamma.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tmin_prec tmin_prec.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tlog tlog.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tlog10 tlog10.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tlog_ui tlog_ui.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tlog1p tlog1p.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tlog2 tlog2.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tminmax tminmax.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tmodf tmodf.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tmul tmul.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tmul_2exp tmul_2exp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tmul_d tmul_d.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tmin_prec tmin_prec.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tmul_ui tmul_ui.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tminmax tminmax.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tmodf tmodf.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tmul_2exp tmul_2exp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tmul_d tmul_d.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tnext tnext.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tmul tmul.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tnrandom tnrandom.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tnrandom_chisq tnrandom_chisq.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tout_str tout_str.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o toutimpl toutimpl.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tmul_ui tmul_ui.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tpow tpow.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tnrandom tnrandom.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tnext tnext.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tnrandom_chisq tnrandom_chisq.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o toutimpl toutimpl.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tpow3 tpow3.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tpow_all tpow_all.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tout_str tout_str.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tpow_z tpow_z.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tprec_round tprec_round.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tprintf tprintf.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tpow tpow.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o trandom trandom.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tpow_all tpow_all.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tpow3 tpow3.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tpow_z tpow_z.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o trandom_deviate trandom_deviate.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tprec_round tprec_round.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tprintf tprintf.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o trec_sqrt trec_sqrt.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tremquo tremquo.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o trint trint.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o trndna trndna.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o trandom trandom.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o trec_sqrt trec_sqrt.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o troot troot.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o trandom_deviate trandom_deviate.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tremquo tremquo.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o trootn_ui trootn_ui.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o trint trint.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o trndna trndna.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsec tsec.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsech tsech.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_d tset_d.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_f tset_f.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o troot troot.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o trootn_ui trootn_ui.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_float128 tset_float128.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsec tsec.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsech tsech.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_d tset_d.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_ld tset_ld.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_f tset_f.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_q tset_q.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_si tset_si.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_sj tset_sj.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_str tset_str.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_float128 tset_float128.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_q tset_q.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_ld tset_ld.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_z tset_z.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_sj tset_sj.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_si tset_si.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tset_z_exp tset_z_exp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_str tset_str.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsi_op tsi_op.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsin tsin.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsin_cos tsin_cos.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsinh tsinh.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_z tset_z.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tset_z_exp tset_z_exp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsi_op tsi_op.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsinh_cosh tsinh_cosh.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsprintf tsprintf.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsqr tsqr.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsin tsin.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsinh tsinh.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsin_cos tsin_cos.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsqrt tsqrt.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsqrt_ui tsqrt_ui.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tstckintc tstckintc.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsinh_cosh tsinh_cosh.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsprintf tsprintf.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tstdint tstdint.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsqr tsqr.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tstrtofr tstrtofr.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsqrt tsqrt.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsub tsub.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsqrt_ui tsqrt_ui.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tstckintc tstckintc.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsub1sp tsub1sp.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsub_d tsub_d.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsub_ui tsub_ui.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tstdint tstdint.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsubnormal tsubnormal.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsub tsub.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tstrtofr tstrtofr.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsub_d tsub_d.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tsum tsum.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsub1sp tsub1sp.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsub_ui tsub_ui.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tswap tswap.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o ttan ttan.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o ttanh ttanh.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o ttotal_order ttotal_order.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsubnormal tsubnormal.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tsum tsum.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o ttrunc ttrunc.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tswap tswap.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o ttan ttan.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tui_div tui_div.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o ttotal_order ttotal_order.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o ttanh ttanh.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tui_pow tui_pow.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tui_sub tui_sub.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o turandom turandom.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tvalist tvalist.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o ttrunc ttrunc.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tui_div tui_div.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tui_pow tui_pow.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o ty0 ty0.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tui_sub tui_sub.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o ty1 ty1.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o turandom turandom.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tyn tyn.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tvalist tvalist.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tzeta tzeta.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
make[2]: 'libfrtests.la' is up to date.
/bin/sh ../libtool  --tag=CC   --mode=link gcc  -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -no-install -L../src/.libs  -o tzeta_ui tzeta_ui.o libfrtests.la -lm  ../src/libmpfr.la -lgmp 
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o ty0 ty0.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o ty1 ty1.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tyn tyn.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tzeta_ui tzeta_ui.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
libtool: link: gcc -Wall -Wmissing-prototypes -Wpointer-arith -O2 -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell -o tzeta tzeta.o  -L../src/.libs ./.libs/libfrtests.a -lm ../src/.libs/libmpfr.so /usr/lib/libgmp.so -Wl,-rpath -Wl,/sources/mpfr-4.1.0/src/.libs
make[2]: Leaving directory '/sources/mpfr-4.1.0/tests'
make  check-TESTS
make[2]: Entering directory '/sources/mpfr-4.1.0/tests'
make[3]: Entering directory '/sources/mpfr-4.1.0/tests'
PASS: tversion
[tversion] MPFR 4.1.0
[tversion] Compiler: GCC 10.2.0
[tversion] C standard: __STDC__ = 1, __STDC_VERSION__ = 201710L
[tversion] __GNUC__ = 10, __GNUC_MINOR__ = 2
[tversion] __GLIBC__ = 2, __GLIBC_MINOR__ = 32
[tversion] GMP: header 6.2.0, library 6.2.0
[tversion] __GMP_CC = "gcc"
[tversion] __GMP_CFLAGS = "-O2 -pedantic -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell"
[tversion] WinDLL: __GMP_LIBGMP_DLL = 0, MPFR_WIN_THREAD_SAFE_DLL = undef
[tversion] MPFR_ALLOCA_MAX = 16384
[tversion] TLS = yes, float128 = yes, decimal = no, GMP internals = no
[tversion] Shared cache = no
[tversion] intmax_t = yes, printf = yes, IEEE floats = yes
[tversion] gmp_printf: hhd = yes, lld = yes, jd = yes, td = yes, Ld = yes
[tversion] _mulx_u64 = yes
[tversion] MPFR tuning parameters from src/x86_64/mparam.h
[tversion] sizeof(long) = 8, sizeof(mpfr_intmax_t) = 8, sizeof(intmax_t) = 8
[tversion] GMP_NUMB_BITS = 64, sizeof(mp_limb_t) = 8
[tversion] Within limb: long = y/y, intmax_t = y/y
[tversion] _MPFR_PREC_FORMAT = 3, sizeof(mpfr_prec_t) = 8
[tversion] _MPFR_EXP_FORMAT = 3, sizeof(mpfr_exp_t) = 8
[tversion] sizeof(mpfr_t) = 32, sizeof(mpfr_ptr) = 8
[tversion] Precision range: [1,9223372036854775551]
[tversion] Max exponent range: [-4611686018427387903,4611686018427387903]
[tversion] Generic ABI code: no
[tversion] Enable formally proven code: no
[tversion] Locale: C
PASS: tassert
PASS: tabort_defalloc1
PASS: tabort_prec_max
PASS: talloc
PASS: tabort_defalloc2
PASS: tinternals
PASS: tinits
PASS: tsgn
PASS: tcheck
PASS: tisnan
PASS: tisqrt
PASS: tset_exp
PASS: mpfr_compat
PASS: mpf_compat
PASS: texceptions
PASS: tacos
PASS: tabs
PASS: tacosh
PASS: tset
PASS: tagm
PASS: tadd_d
PASS: talloc-cache
PASS: tadd_ui
PASS: tasin
PASS: tai
PASS: tadd1sp
PASS: tasinh
PASS: tatanh
PASS: tbuildopt
PASS: tatan
PASS: tcbrt
PASS: tcan_round
PASS: tcmp
PASS: tcmp_d
PASS: tcmp_ld
PASS: tcmp_ui
PASS: tcmpabs
PASS: tcmp2
PASS: tconst_catalan
PASS: tconst_euler
PASS: taway
PASS: tconst_pi
PASS: tconst_log2
PASS: tcomparisons
PASS: tcopysign
PASS: tcot
PASS: tcos
PASS: tcosh
PASS: tcoth
PASS: tcsch
PASS: tcsc
PASS: reuse
PASS: tdim
PASS: td_sub
PASS: td_div
PASS: tdiv_ui
PASS: tdot
PASS: tdiv_d
PASS: teq
PASS: terandom
PASS: terandom_chisq
PASS: teint
PASS: tadd
PASS: texp
PASS: tdigamma
PASS: texp2
PASS: texp10
PASS: texpm1
PASS: tfits
PASS: tfma
PASS: tfmod
PASS: tfactorial
PASS: tfpif
PASS: tbeta
PASS: tfms
PASS: tfrexp
PASS: tdiv
PASS: tfprintf
PASS: tget_d
PASS: tgamma
PASS: tget_d_2exp
PASS: tget_flt
PASS: tget_f
PASS: tget_ld_2exp
PASS: tget_q
SKIP: tget_set_d64
SKIP: tget_set_d128
PASS: tfrac
PASS: tfmma
PASS: tget_sj
PASS: tget_z
PASS: thyperbolic
PASS: thypot
PASS: tgrandom
PASS: tinp_str
PASS: tj1
PASS: tj0
PASS: tl2b
PASS: tgmpop
PASS: tlgamma
PASS: tli2
PASS: tjn
PASS: tlngamma
PASS: tlog10
PASS: tlog1p
PASS: tlog
PASS: tmin_prec
PASS: tlog2
PASS: tminmax
PASS: tmodf
PASS: tmul_2exp
PASS: tmul_d
PASS: tgamma_inc
PASS: tnext
PASS: tnrandom
PASS: tmul_ui
PASS: tout_str
PASS: toutimpl
PASS: tmul
PASS: tpow3
PASS: tpow_all
PASS: tpow_z
PASS: tprec_round
PASS: tnrandom_chisq
PASS: trandom
PASS: trandom_deviate
PASS: tprintf
PASS: tpow
PASS: tremquo
PASS: trndna
PASS: trint
PASS: terf
PASS: troot
PASS: tsech
PASS: trootn_ui
PASS: tsec
PASS: tset_f
PASS: tset_d
PASS: tset_q
PASS: tset_ld
PASS: tset_sj
PASS: tset_si
PASS: tset_str
PASS: tset_z
PASS: tset_z_exp
PASS: tsi_op
PASS: tsin_cos
PASS: tlog_ui
PASS: tsinh_cosh
PASS: tsinh
PASS: tsin
PASS: tsqr
PASS: tsqrt_ui
PASS: tstckintc
PASS: tstdint
PASS: tsprintf
PASS: trec_sqrt
PASS: tsub1sp
PASS: tstrtofr
PASS: tsqrt
PASS: tsubnormal
PASS: tset_float128
PASS: tswap
PASS: tsub_d
PASS: tsub_ui
PASS: ttotal_order
PASS: ttrunc
PASS: ttanh
PASS: tui_pow
PASS: ttan
PASS: tui_sub
PASS: tvalist
PASS: tui_div
PASS: ty0
PASS: turandom
PASS: tyn
PASS: ty1
PASS: tzeta_ui
PASS: tsub
PASS: tzeta
PASS: tget_str
PASS: tsum
============================================================================
Testsuite summary for MPFR 4.1.0
============================================================================
# TOTAL: 183
# PASS:  181
# SKIP:  2
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================
make[3]: Leaving directory '/sources/mpfr-4.1.0/tests'
make[2]: Leaving directory '/sources/mpfr-4.1.0/tests'
[tversion] MPFR 4.1.0
[tversion] Compiler: GCC 10.2.0
[tversion] C standard: __STDC__ = 1, __STDC_VERSION__ = 201710L
[tversion] __GNUC__ = 10, __GNUC_MINOR__ = 2
[tversion] __GLIBC__ = 2, __GLIBC_MINOR__ = 32
[tversion] GMP: header 6.2.0, library 6.2.0
[tversion] __GMP_CC = "gcc"
[tversion] __GMP_CFLAGS = "-O2 -pedantic -fomit-frame-pointer -m64 -mtune=skylake -march=broadwell"
[tversion] WinDLL: __GMP_LIBGMP_DLL = 0, MPFR_WIN_THREAD_SAFE_DLL = undef
[tversion] MPFR_ALLOCA_MAX = 16384
[tversion] TLS = yes, float128 = yes, decimal = no, GMP internals = no
[tversion] Shared cache = no
[tversion] intmax_t = yes, printf = yes, IEEE floats = yes
[tversion] gmp_printf: hhd = yes, lld = yes, jd = yes, td = yes, Ld = yes
[tversion] _mulx_u64 = yes
[tversion] MPFR tuning parameters from src/x86_64/mparam.h
[tversion] sizeof(long) = 8, sizeof(mpfr_intmax_t) = 8, sizeof(intmax_t) = 8
[tversion] GMP_NUMB_BITS = 64, sizeof(mp_limb_t) = 8
[tversion] Within limb: long = y/y, intmax_t = y/y
[tversion] _MPFR_PREC_FORMAT = 3, sizeof(mpfr_prec_t) = 8
[tversion] _MPFR_EXP_FORMAT = 3, sizeof(mpfr_exp_t) = 8
[tversion] sizeof(mpfr_t) = 32, sizeof(mpfr_ptr) = 8
[tversion] Precision range: [1,9223372036854775551]
[tversion] Max exponent range: [-4611686018427387903,4611686018427387903]
[tversion] Generic ABI code: no
[tversion] Enable formally proven code: no
[tversion] Locale: C
make[1]: Leaving directory '/sources/mpfr-4.1.0/tests'
Making check in tune
make[1]: Entering directory '/sources/mpfr-4.1.0/tune'
make[1]: Nothing to be done for 'check'.
make[1]: Leaving directory '/sources/mpfr-4.1.0/tune'
Making check in tools/bench
make[1]: Entering directory '/sources/mpfr-4.1.0/tools/bench'
make[1]: Nothing to be done for 'check'.
make[1]: Leaving directory '/sources/mpfr-4.1.0/tools/bench'
make[1]: Entering directory '/sources/mpfr-4.1.0'
make[1]: Nothing to be done for 'check-am'.
make[1]: Leaving directory '/sources/mpfr-4.1.0'

real	0m15.442s
user	1m12.167s
sys	0m6.399s
(lfs chroot) root:/sources/mpfr-4.1.0# 
(lfs chroot) root:/sources/mpfr-4.1.0#

===== TL;DR =====
```

<br>
### INPUT
```
time make install
time make install-html
cd ../
rm -rf mpfr-4.1.0/

```

(lfs chroot) root:/sources/mpfr-4.1.0# time make install
Making install in doc
make[1]: Entering directory '/sources/mpfr-4.1.0/doc'
make[2]: Entering directory '/sources/mpfr-4.1.0/doc'
make[2]: Nothing to be done for 'install-exec-am'.
 /bin/mkdir -p '/usr/share/doc/mpfr-4.1.0'
 /bin/mkdir -p '/usr/share/info'
 /usr/bin/install -c -m 644 ./mpfr.info '/usr/share/info'
 /usr/bin/install -c -m 644 FAQ.html '/usr/share/doc/mpfr-4.1.0'
 install-info --info-dir='/usr/share/info' '/usr/share/info/mpfr.info'
make[2]: Leaving directory '/sources/mpfr-4.1.0/doc'
make[1]: Leaving directory '/sources/mpfr-4.1.0/doc'
Making install in src
make[1]: Entering directory '/sources/mpfr-4.1.0/src'
make  install-am
make[2]: Entering directory '/sources/mpfr-4.1.0/src'
make[3]: Entering directory '/sources/mpfr-4.1.0/src'
 /bin/mkdir -p '/usr/lib'
 /bin/mkdir -p '/usr/include'
 /bin/sh ../libtool   --mode=install /usr/bin/install -c   libmpfr.la '/usr/lib'
 /usr/bin/install -c -m 644 mpfr.h mpf2mpfr.h '/usr/include'
libtool: install: /usr/bin/install -c .libs/libmpfr.so.6.1.0 /usr/lib/libmpfr.so.6.1.0
libtool: install: (cd /usr/lib && { ln -s -f libmpfr.so.6.1.0 libmpfr.so.6 || { rm -f libmpfr.so.6 && ln -s libmpfr.so.6.1.0 libmpfr.so.6; }; })
libtool: install: (cd /usr/lib && { ln -s -f libmpfr.so.6.1.0 libmpfr.so || { rm -f libmpfr.so && ln -s libmpfr.so.6.1.0 libmpfr.so; }; })
libtool: install: /usr/bin/install -c .libs/libmpfr.lai /usr/lib/libmpfr.la
libtool: finish: PATH="/bin:/usr/bin:/sbin:/usr/sbin:/sbin" ldconfig -n /usr/lib
----------------------------------------------------------------------
Libraries have been installed in:
   /usr/lib

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the '-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the 'LD_RUN_PATH' environment variable
     during linking
   - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to '/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------
make[3]: Leaving directory '/sources/mpfr-4.1.0/src'
make[2]: Leaving directory '/sources/mpfr-4.1.0/src'
make[1]: Leaving directory '/sources/mpfr-4.1.0/src'
Making install in tests
make[1]: Entering directory '/sources/mpfr-4.1.0/tests'
make[2]: Entering directory '/sources/mpfr-4.1.0/tests'
make[2]: Nothing to be done for 'install-exec-am'.
make[2]: Nothing to be done for 'install-data-am'.
make[2]: Leaving directory '/sources/mpfr-4.1.0/tests'
make[1]: Leaving directory '/sources/mpfr-4.1.0/tests'
Making install in tune
make[1]: Entering directory '/sources/mpfr-4.1.0/tune'
make[2]: Entering directory '/sources/mpfr-4.1.0/tune'
make[2]: Nothing to be done for 'install-exec-am'.
make[2]: Nothing to be done for 'install-data-am'.
make[2]: Leaving directory '/sources/mpfr-4.1.0/tune'
make[1]: Leaving directory '/sources/mpfr-4.1.0/tune'
Making install in tools/bench
make[1]: Entering directory '/sources/mpfr-4.1.0/tools/bench'
make[2]: Entering directory '/sources/mpfr-4.1.0/tools/bench'
make[2]: Nothing to be done for 'install-exec-am'.
make[2]: Nothing to be done for 'install-data-am'.
make[2]: Leaving directory '/sources/mpfr-4.1.0/tools/bench'
make[1]: Leaving directory '/sources/mpfr-4.1.0/tools/bench'
make[1]: Entering directory '/sources/mpfr-4.1.0'
make[2]: Entering directory '/sources/mpfr-4.1.0'
make[2]: Nothing to be done for 'install-exec-am'.
 /bin/mkdir -p '/usr/share/doc/mpfr-4.1.0'
 /bin/mkdir -p '/usr/lib/pkgconfig'
 /usr/bin/install -c -m 644 mpfr.pc '/usr/lib/pkgconfig'
 /bin/mkdir -p '/usr/share/doc/mpfr-4.1.0/examples'
 /usr/bin/install -c -m 644  examples/ReadMe examples/can_round.c examples/divworst.c examples/rndo-add.c examples/sample.c examples/threads.c examples/version.c '/usr/share/doc/mpfr-4.1.0/examples'
 /usr/bin/install -c -m 644  AUTHORS BUGS COPYING COPYING.LESSER NEWS TODO '/usr/share/doc/mpfr-4.1.0/.'
make[2]: Leaving directory '/sources/mpfr-4.1.0'
make[1]: Leaving directory '/sources/mpfr-4.1.0'

real	0m0.759s
user	0m0.343s
sys	0m0.088s
(lfs chroot) root:/sources/mpfr-4.1.0# time make install-html
Making install-html in doc
make[1]: Entering directory '/sources/mpfr-4.1.0/doc'
 /bin/mkdir -p '/usr/share/doc/mpfr-4.1.0'
 /bin/mkdir -p '/usr/share/doc/mpfr-4.1.0/mpfr.html'
 /usr/bin/install -c -m 644 'mpfr.html'/* '/usr/share/doc/mpfr-4.1.0/mpfr.html'
make[1]: Leaving directory '/sources/mpfr-4.1.0/doc'
Making install-html in src
make[1]: Entering directory '/sources/mpfr-4.1.0/src'
make[1]: Nothing to be done for 'install-html'.
make[1]: Leaving directory '/sources/mpfr-4.1.0/src'
Making install-html in tests
make[1]: Entering directory '/sources/mpfr-4.1.0/tests'
make[1]: Nothing to be done for 'install-html'.
make[1]: Leaving directory '/sources/mpfr-4.1.0/tests'
Making install-html in tune
make[1]: Entering directory '/sources/mpfr-4.1.0/tune'
make[1]: Nothing to be done for 'install-html'.
make[1]: Leaving directory '/sources/mpfr-4.1.0/tune'
Making install-html in tools/bench
make[1]: Entering directory '/sources/mpfr-4.1.0/tools/bench'
make[1]: Nothing to be done for 'install-html'.
make[1]: Leaving directory '/sources/mpfr-4.1.0/tools/bench'
make[1]: Entering directory '/sources/mpfr-4.1.0'
make[1]: Nothing to be done for 'install-html-am'.
make[1]: Leaving directory '/sources/mpfr-4.1.0'

real	0m0.073s
user	0m0.067s
sys	0m0.006s
(lfs chroot) root:/sources/mpfr-4.1.0# 
(lfs chroot) root:/sources/mpfr-4.1.0# cd ../
(lfs chroot) root:/sources# rm -rf mpfr-4.1.0/
(lfs chroot) root:/sources# 


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

