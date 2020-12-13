---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-11.md)
[NEXT](index.md)

<br>
<span style="color:red; font-weight:bold; font-size:larger;">
It is assumed that you understand how install a Debian VirtualBox Guest.
If you have never installed a VirtualBox Guest before, visit [OSP4DISS](https://osp4diss.vlsm.org/).
</span>

<br>
# LFS: Chapter 8 part 2

## Virtual Box Guest LFS-08-part2

* Import LFS-08.ova, rename to LFS-08-part2

<br>
<img src="pictures/LFS-A45.jpg" width="960">

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
cd /sources/

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

(lfs chroot) root:~# cd /sources/

(lfs chroot) root:/sources#

```

<br>
# Autoconf-2.69

### INPUT
```
tar xf autoconf-2.69.tar.xz
cd autoconf-2.69/
sed -i '361 s/{/\\{/' bin/autoscan.in
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf autoconf-2.69.tar.xz

(lfs chroot) root:/sources# cd autoconf-2.69/

(lfs chroot) root:/sources/autoconf-2.69# sed -i '361 s/{/\\{/' bin/autoscan.in

(lfs chroot) root:/sources/autoconf-2.69# ./configure --prefix=/usr
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: creating lib/autotest/Makefile
config.status: creating bin/Makefile
config.status: executing tests/atconfig commands

(lfs chroot) root:/sources/autoconf-2.69# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/autoconf-2.69# time make
make  all-recursive
make[1]: Entering directory '/sources/autoconf-2.69'
Making all in bin

===== TL;DR =====

real	0m1.050s
user	0m0.899s
sys	0m0.219s

(lfs chroot) root:/sources/autoconf-2.69#

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/autoconf-2.69# time make check
if test -d ./.git; then			\
  cd . &&						\
  git submodule --quiet foreach test '$(git rev-parse $sha1)'	\

===== TL;DR =====

/bin/sh ./testsuite 
## ----------------------------- ##
## GNU Autoconf 2.69 test suite. ##
## ----------------------------- ##

===== TL;DR =====

## ------------- ##
## Test results. ##
## ------------- ##

ERROR: 450 tests were run,
137 failed (4 expected failures).
53 tests were skipped.
## -------------------------- ##
## testsuite.log was created. ##
## -------------------------- ##

Please send `tests/testsuite.log' and all information you think might help:

   To: <bug-autoconf@gnu.org>
   Subject: [GNU Autoconf 2.69] testsuite: 77 232 233 270 272 273 274 276 277 281 282 287 288 289 290 291 292 293 294 295 296 297 298 299 300 301 302 303 348 349 353 354 355 356 357 358 362 363 364 365 366 367 368 369 370 371 372 374 380 381 382 383 384 385 386 387 388 389 390 391 404 405 406 407 418 419 420 421 422 423 424 425 426 427 431 432 433 434 435 436 437 438 443 444 445 446 447 448 449 450 451 452 453 454 455 456 457 458 459 460 461 462 463 471 472 473 474 475 476 477 478 479 480 481 482 483 484 485 486 487 488 489 490 491 492 493 494 495 496 497 498 499 501 failed

===== TL;DR =====

real	9m9.766s
user	6m25.861s
sys	1m52.105s

(lfs chroot) root:/sources/autoconf-2.69# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/autoconf-2.69# time make install
make  install-recursive
make[1]: Entering directory '/sources/autoconf-2.69'
Making install in bin

===== TL;DR =====

real	0m0.584s
user	0m0.342s
sys	0m0.238s

(lfs chroot) root:/sources/autoconf-2.69# 

```

<br>
### INPUT
```
cd ../
rm -rf autoconf-2.69/

```

### OUTPUT
```
(lfs chroot) root:/sources/autoconf-2.69# cd ../

(lfs chroot) root:/sources# rm -rf autoconf-2.69/

(lfs chroot) root:/sources# 

```

<br>
# Automake-1.16.2

### INPUT
```
tar xf automake-1.16.2.tar.xz
cd automake-1.16.2/
sed -i "s/''/etags/" t/tags-lisp-space.sh
./configure --prefix=/usr --docdir=/usr/share/doc/automake-1.16.2

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf automake-1.16.2.tar.xz

(lfs chroot) root:/sources# cd automake-1.16.2/

(lfs chroot) root:/sources/automake-1.16.2# sed -i "s/''/etags/" t/tags-lisp-space.sh

(lfs chroot) root:/sources/automake-1.16.2# ./configure --prefix=/usr --docdir=/usr/share/doc/automake-1.16.2
checking whether make supports nested variables... yes
checking build system type... x86_64-pc-linux-gnu
checking host system type... x86_64-pc-linux-gnu

===== TL;DR =====

configure: creating ./config.status
config.status: creating Makefile
config.status: creating pre-inst-env

(lfs chroot) root:/sources/automake-1.16.2#

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/automake-1.16.2# time make
  GEN      bin/automake
  GEN      bin/aclocal
  GEN      t/ax/shell-no-trail-bslash
  GEN      t/ax/cc-no-c-o
  GEN      doc/aclocal.1
  GEN      runtest
  GEN      doc/automake.1
  GEN      lib/Automake/Config.pm
  GEN      bin/aclocal-1.16
  GEN      doc/aclocal-1.16.1
  GEN      t/ax/test-defs.sh
  GEN      bin/automake-1.16
  GEN      doc/automake-1.16.1

real	0m0.347s
user	0m0.657s
sys	0m0.115s

(lfs chroot) root:/sources/automake-1.16.2# 

```

<br>
### INPUT
```
time make -j8 check

```

### OUTPUT
```
(lfs chroot) root:/sources/automake-1.16.2# time make -j8 check
make  check-TESTS check-local
make[1]: Entering directory '/sources/automake-1.16.2'
make[2]: Entering directory '/sources/automake-1.16.2'
XFAIL: t/pm/Cond2.pl

===== TL;DR =====

============================================================================
Testsuite summary for GNU Automake 1.16.2
============================================================================
# TOTAL: 2915
# PASS:  2718
# SKIP:  158
# XFAIL: 39
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================
make[2]: Leaving directory '/sources/automake-1.16.2'
make[1]: Leaving directory '/sources/automake-1.16.2'

real	9m40.859s
user	28m35.857s
sys	5m33.765s

(lfs chroot) root:/sources/automake-1.16.2# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/automake-1.16.2# time make install
make[1]: Entering directory '/sources/automake-1.16.2'
 /bin/mkdir -p '/usr/bin'
 /bin/mkdir -p '/usr/share/automake-1.16/am'

===== TL;DR =====

 chmod +x '/usr/share/automake-1.16/tap-driver.sh'
make[2]: Leaving directory '/sources/automake-1.16.2'
make[1]: Leaving directory '/sources/automake-1.16.2'

real	0m0.167s
user	0m0.222s
sys	0m0.074s

(lfs chroot) root:/sources/automake-1.16.2#

```

<br>
### INPUT
```
cd ../
rm -rf automake-1.16.2/

```

### OUTPUT
```
(lfs chroot) root:/sources/automake-1.16.2# cd ../

(lfs chroot) root:/sources# rm -rf automake-1.16.2/

(lfs chroot) root:/sources# 

```

<br>
# Kmod-27

### INPUT
```
tar xf kmod-27.tar.xz
cd kmod-27/
./configure --prefix=/usr          \
            --bindir=/bin          \
            --sysconfdir=/etc      \
            --with-rootlibdir=/lib \
            --with-xz              \
            --with-zlib

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf kmod-27.tar.xz

(lfs chroot) root:/sources# cd kmod-27/

(lfs chroot) root:/sources/kmod-27# ./configure --prefix=/usr          \
>             --bindir=/bin          \
>             --sysconfdir=/etc      \
>             --with-rootlibdir=/lib \

===== TL;DR =====

	kmod 27
	=======

	prefix:			/usr
	sysconfdir:		/etc
	libdir:			${exec_prefix}/lib
	rootlibdir:		/lib
	includedir:		${prefix}/include
	bindir:			/bin
	Bash completions dir:   ${datarootdir}/bash-completion/completions

	compiler:		gcc
	cflags:			 -pipe -DANOTHER_BRICK_IN_THE -Wall -W -Wextra -Wno-inline -Wvla -Wundef -Wformat=2 -Wlogical-op -Wsign-compare -Wmissing-include-dirs -Wold-style-definition -Wpointer-arith -Winit-self -Wdeclaration-after-statement -Wfloat-equal -Wmissing-prototypes -Wstrict-prototypes -Wredundant-decls -Wmissing-declarations -Wmissing-noreturn -Wshadow -Wendif-labels -Wstrict-aliasing=3 -Wwrite-strings -Wno-long-long -Wno-overlength-strings -Wno-unused-parameter -Wno-missing-field-initializers -Wno-unused-result -Wnested-externs -Wchar-subscripts -Wtype-limits -Wuninitialized -fno-common -fdiagnostics-show-option -fvisibility=hidden -ffunction-sections -fdata-sections -g -O2
	ldflags:		 -Wl,--as-needed -Wl,--no-undefined -Wl,--gc-sections 

	experimental features:  no
	tools:			yes
	python bindings:	no
	logging:		yes
	compression:		xz=yes  zlib=yes
	debug:			no
	coverage:		no
	doc:			
	man:			yes
	test-modules:           yes

	features:               +XZ +ZLIB -LIBCRYPTO -EXPERIMENTAL

(lfs chroot) root:/sources/kmod-27# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/kmod-27# time make
make --no-print-directory all-recursive
Making all in .
  GEN      libkmod/libkmod.pc

===== TL;DR =====

real	0m1.472s
user	0m5.102s
sys	0m0.529s

(lfs chroot) root:/sources/kmod-27# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/kmod-27# time make install
Making install in .
 /bin/mkdir -p '/usr/share/bash-completion/completions'
 /bin/mkdir -p '/usr/include'

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

===== TL;DR =====

real	0m0.431s
user	0m0.244s
sys	0m0.072s

(lfs chroot) root:/sources/kmod-27#

```

<br>
### INPUT
```
for target in depmod insmod lsmod modinfo modprobe rmmod; do
  ln -sfv ../bin/kmod /sbin/$target
done
ln -sfv kmod /bin/lsmod

```

### OUTPUT
```
(lfs chroot) root:/sources/kmod-27# for target in depmod insmod lsmod modinfo modprobe rmmod; do
>   ln -sfv ../bin/kmod /sbin/$target
> done
'/sbin/depmod' -> '../bin/kmod'
'/sbin/insmod' -> '../bin/kmod'
'/sbin/lsmod' -> '../bin/kmod'
'/sbin/modinfo' -> '../bin/kmod'
'/sbin/modprobe' -> '../bin/kmod'
'/sbin/rmmod' -> '../bin/kmod'
(lfs chroot) root:/sources/kmod-27# ln -sfv kmod /bin/lsmod
'/bin/lsmod' -> 'kmod'

(lfs chroot) root:/sources/kmod-27# 

```

<br>
### INPUT
```
cd ../
rm -rf kmod-27/

```

### OUTPUT
```
(lfs chroot) root:/sources/kmod-27# cd ../

(lfs chroot) root:/sources# rm -rf kmod-27/

(lfs chroot) root:/sources# 

```

<br>
# Libelf from Elfutils-0.180

### INPUT
```
tar xf elfutils-0.180.tar.bz2
cd elfutils-0.180/
./configure --prefix=/usr --disable-debuginfod --libdir=/lib

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf elfutils-0.180.tar.bz2

(lfs chroot) root:/sources# cd elfutils-0.180/

(lfs chroot) root:/sources/elfutils-0.180# ./configure --prefix=/usr --disable-debuginfod --libdir=/lib
configure: No --program-prefix given, using "eu-"
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes

===== TL;DR =====

configure:
=====================================================================
        elfutils: 0.180 (eu_version: 180)
=====================================================================

    Prefix                             : /usr
    Program prefix ("eu-" recommended) : eu-
    Source code location               : .
    Maintainer mode                    : 
    build arch                         : x86_64-pc-linux-gnu

  RECOMMENDED FEATURES (should all be yes)
    gzip support                       : yes
    bzip2 support                      : yes
    lzma/xz support                    : yes
    libstdc++ demangle support         : yes
    File textrel check                 : yes
    Symbol versioning                  : yes

  NOT RECOMMENDED FEATURES (should all be no)
    Experimental thread safety         : no
    install elf.h                      : no

  OTHER FEATURES
    Deterministic archives by default  : false
    Native language support            : yes
    Extra Valgrind annotations         : no
    Debuginfod client/server support   : no

  EXTRA TEST FEATURES (used with make check)
    have bunzip2 installed (required)  : yes
    debug branch prediction            : no
    gprof support                      : no
    gcov support                       : no
    run all tests under valgrind       : no
    gcc undefined behaviour sanitizer  : no
    use rpath in tests                 : no
    test biarch                        : no

(lfs chroot) root:/sources/elfutils-0.180# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/elfutils-0.180# time make
make --no-print-directory all-recursive
Making all in config
make[2]: Nothing to be done for 'all'.

===== TL;DR =====

real	0m18.214s
user	1m11.700s
sys	0m11.617s

(lfs chroot) root:/sources/elfutils-0.180#

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/elfutils-0.180# time make check
Making check in config
make[1]: Nothing to be done for 'check'.
Making check in m4

===== TL;DR =====

============================================================================
Testsuite summary for elfutils 0.180
============================================================================
# TOTAL: 218
# PASS:  214
# SKIP:  4
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

real	0m26.269s
user	0m26.929s
sys	0m9.549s

```

<br>
### INPUT
```
make -C libelf install
install -vm644 config/libelf.pc /usr/lib/pkgconfig
rm /lib/libelf.a

```

### OUTPUT
```
(lfs chroot) root:/sources/elfutils-0.180# make -C libelf install
make: Entering directory '/sources/elfutils-0.180/libelf'
make[1]: Entering directory '/sources/elfutils-0.180/libelf'
 /bin/mkdir -p '/lib'
 /bin/mkdir -p '/usr/include'
 /bin/mkdir -p '/usr/include/elfutils'
 /usr/bin/install -c -m 644  libelf.a '/lib'
 /usr/bin/install -c -m 644 elf-knowledge.h '/usr/include/elfutils'
 /usr/bin/install -c -m 644 libelf.h gelf.h nlist.h '/usr/include'
 ( cd '/lib' && ranlib libelf.a )
make[1]: Leaving directory '/sources/elfutils-0.180/libelf'
/bin/sh /sources/elfutils-0.180/config/install-sh -d /lib
/usr/bin/install -c libelf.so /lib/libelf-0.180.so
ln -fs libelf-0.180.so /lib/libelf.so.1
ln -fs libelf.so.1 /lib/libelf.so
make: Leaving directory '/sources/elfutils-0.180/libelf'

(lfs chroot) root:/sources/elfutils-0.180# install -vm644 config/libelf.pc /usr/lib/pkgconfig
'config/libelf.pc' -> '/usr/lib/pkgconfig/libelf.pc'

(lfs chroot) root:/sources/elfutils-0.180# rm /lib/libelf.a

(lfs chroot) root:/sources/elfutils-0.180# 

```

<br>
### INPUT
```
cd ../
rm -rf elfutils-0.180/

```

### OUTPUT
```
(lfs chroot) root:/sources/elfutils-0.180# cd ../

(lfs chroot) root:/sources# rm -rf elfutils-0.180/

(lfs chroot) root:/sources# 

```

<br>
# Libffi-3.3

### INPUT
```
tar xf libffi-3.3.tar.gz
cd libffi-3.3/
./configure --prefix=/usr --disable-static --with-gcc-arch=native

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
time make check

```

### OUTPUT
```

===== TL;DR =====
```

<br>
### INPUT
```
time make install

```

### OUTPUT
```

===== TL;DR =====
```

<br>
### INPUT
```

cd ../
rm -rf 

```

### OUTPUT
```

===== TL;DR =====
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

<br>
### INPUT

```
exit

```

### OUTPUT
```
(lfs chroot) root:/sources# exit
logout

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

* Create LFS-08-part2.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-11.md)
[NEXT](index.md)
<br>

