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
# XXX

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

