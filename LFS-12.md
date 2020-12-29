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
alias cl='clear;echo ""'
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
(lfs chroot) root:/sources# tar xf libffi-3.3.tar.gz

(lfs chroot) root:/sources# cd libffi-3.3/

(lfs chroot) root:/sources/libffi-3.3# ./configure --prefix=/usr --disable-static --with-gcc-arch=native
checking build system type... x86_64-pc-linux-gnu
checking host system type... x86_64-pc-linux-gnu
checking target system type... x86_64-pc-linux-gnu

===== TL;DR =====

config.status: executing libtool commands
config.status: executing include commands
config.status: executing src commands

(lfs chroot) root:/sources/libffi-3.3# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/libffi-3.3# time make
MAKE x86_64-pc-linux-gnu : 0 * all-all
make[1]: Entering directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'
make  all-recursive
make[2]: Entering directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'

===== TL;DR =====

make[3]: Leaving directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'
make[2]: Leaving directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'
make[1]: Leaving directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'

real	0m0.649s
user	0m1.306s
sys	0m0.164s

(lfs chroot) root:/sources/libffi-3.3# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/libffi-3.3# time make check
MAKE x86_64-pc-linux-gnu : 0 * check
make[1]: Entering directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'
Making check in include

===== TL;DR =====

		=== libffi tests ===

===== TL;DR =====

		=== libffi Summary ===

# of expected passes		2284

===== TL;DR =====

make[2]: Entering directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'
make[2]: Leaving directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'
make[1]: Leaving directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'

real	3m26.182s
user	3m7.521s
sys	0m16.970s

(lfs chroot) root:/sources/libffi-3.3# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/libffi-3.3# time make install
MAKE x86_64-pc-linux-gnu : 0 * install
make[1]: Entering directory '/sources/libffi-3.3/x86_64-pc-linux-gnu'
Making install in include

===== TL;DR =====

----------------------------------------------------------------------
Libraries have been installed in:
   /usr/lib/../lib

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

real	0m0.331s
user	0m0.248s
sys	0m0.081s

(lfs chroot) root:/sources/libffi-3.3# 

```

<br>
### INPUT
```
cd ../
rm -rf libffi-3.3/

```

### OUTPUT
```
(lfs chroot) root:/sources/libffi-3.3# cd ../

(lfs chroot) root:/sources# rm -rf libffi-3.3/

(lfs chroot) root:/sources# 

```

<br>
# OpenSSL-1.1.1g

### INPUT
```
tar xf openssl-1.1.1g.tar.gz
cd openssl-1.1.1g/
./config --prefix=/usr         \
         --openssldir=/etc/ssl \
         --libdir=lib          \
         shared                \
         zlib-dynamic

```

### OUTPUT
```
(lfs chroot) root:/sources/openssl-1.1.1g# tar xf openssl-1.1.1g.tar.gz
tar: openssl-1.1.1g.tar.gz: Cannot open: No such file or directory
tar: Error is not recoverable: exiting now
(lfs chroot) root:/sources/openssl-1.1.1g# cd openssl-1.1.1g/
bash: cd: openssl-1.1.1g/: No such file or directory

(lfs chroot) root:/sources/openssl-1.1.1g# ./config --prefix=/usr         \
>          --openssldir=/etc/ssl \
>          --libdir=lib          \
>          shared                \
>          zlib-dynamic

===== TL;DR =====

**********************************************************************
***                                                                ***
***   OpenSSL has been successfully configured                     ***
***                                                                ***
***   If you encounter a problem while building, please open an    ***
***   issue on GitHub <https://github.com/openssl/openssl/issues>  ***
***   and include the output from the following command:           ***
***                                                                ***
***       perl configdata.pm --dump                                ***
***                                                                ***
***   (If you are new to OpenSSL, you might want to consult the    ***
***   'Troubleshooting' section in the INSTALL file first)         ***
***                                                                ***
**********************************************************************

===== TL;DR =====

(lfs chroot) root:/sources/openssl-1.1.1g# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/openssl-1.1.1g# time make
/usr/bin/perl "-I." -Mconfigdata "util/dofile.pl" \
    "-oMakefile" include/crypto/bn_conf.h.in > include/crypto/bn_conf.h
/usr/bin/perl "-I." -Mconfigdata "util/dofile.pl" \
    "-oMakefile" include/crypto/dso_conf.h.in > include/crypto/dso_conf.h

===== TL;DR =====

real	0m25.300s
user	2m8.937s
sys	0m15.934s

(lfs chroot) root:/sources/openssl-1.1.1g# 

```

<br>
### INPUT
```
time make test

```

### OUTPUT
```
(lfs chroot) root:/sources/openssl-1.1.1g# time make test
make depend && make _tests
make[1]: Entering directory '/sources/openssl-1.1.1g'
make[1]: Leaving directory '/sources/openssl-1.1.1g'

===== TL;DR =====

All tests successful.
Files=155, Tests=1468, 97 wallclock secs ( 1.35 usr  0.31 sys + 86.44 cusr 20.35 csys = 108.45 CPU)
Result: PASS
make[1]: Leaving directory '/sources/openssl-1.1.1g'

real	1m36.605s
user	1m28.040s
sys	0m20.681s

(lfs chroot) root:/sources/openssl-1.1.1g# 

```

<br>
### INPUT
```
sed -i '/INSTALL_LIBS/s/libcrypto.a libssl.a//' Makefile
time make MANSUFFIX=ssl install

```

### OUTPUT
```
(lfs chroot) root:/sources/openssl-1.1.1g# sed -i '/INSTALL_LIBS/s/libcrypto.a libssl.a//' Makefile

(lfs chroot) root:/sources/openssl-1.1.1g# time make MANSUFFIX=ssl install
make depend && make _build_libs
make depend && make _build_engines
make depend && make _build_programs

===== TL;DR =====

real	0m26.951s
user	0m47.106s
sys	0m5.351s

(lfs chroot) root:/sources/openssl-1.1.1g# 

```

<br>
### INPUT
```
mv -v /usr/share/doc/openssl /usr/share/doc/openssl-1.1.1g
cp -vfr doc/* /usr/share/doc/openssl-1.1.1g

```

### OUTPUT
```
(lfs chroot) root:/sources/openssl-1.1.1g# mv -v /usr/share/doc/openssl /usr/share/doc/openssl-1.1.1g
renamed '/usr/share/doc/openssl' -> '/usr/share/doc/openssl-1.1.1g'

(lfs chroot) root:/sources/openssl-1.1.1g# cp -vfr doc/* /usr/share/doc/openssl-1.1.1g
'doc/HOWTO' -> '/usr/share/doc/openssl-1.1.1g/HOWTO'
'doc/HOWTO/certificates.txt' -> '/usr/share/doc/openssl-1.1.1g/HOWTO/certificates.txt'
'doc/HOWTO/keys.txt' -> '/usr/share/doc/openssl-1.1.1g/HOWTO/keys.txt'

===== TL;DR =====

'doc/man7/ssl.pod' -> '/usr/share/doc/openssl-1.1.1g/man7/ssl.pod'
'doc/man7/x509.pod' -> '/usr/share/doc/openssl-1.1.1g/man7/x509.pod'
'doc/openssl-c-indent.el' -> '/usr/share/doc/openssl-1.1.1g/openssl-c-indent.el'

(lfs chroot) root:/sources/openssl-1.1.1g# 

```

<br>
### INPUT
```
cd ../
rm -rf openssl-1.1.1g/

```

### OUTPUT
```
(lfs chroot) root:/sources/openssl-1.1.1g# cd ../

(lfs chroot) root:/sources# rm -rf openssl-1.1.1g/

(lfs chroot) root:/sources# 

```

<br>
# Python-3.8.5

### INPUT
```
tar xf Python-3.8.5.tar.xz
cd Python-3.8.5/
./configure --prefix=/usr       \
            --enable-shared     \
            --with-system-expat \
            --with-system-ffi   \
            --with-ensurepip=yes

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf Python-3.8.5.tar.xz

(lfs chroot) root:/sources# cd Python-3.8.5/

(lfs chroot) root:/sources/Python-3.8.5# ./configure --prefix=/usr       \
>             --enable-shared     \
>             --with-system-expat \
>             --with-system-ffi   \
>             --with-ensurepip=yes

===== TL;DR =====

config.status: creating pyconfig.h
creating Modules/Setup.local
creating Makefile


If you want a release build with all stable optimizations active (PGO, etc),
please run ./configure --enable-optimizations

(lfs chroot) root:/sources/Python-3.8.5# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/Python-3.8.5# time make
gcc -pthread -c -Wno-unused-result -Wsign-compare -DNDEBUG -g -fwrapv -O3 -Wall    -std=c99 -Wextra -Wno-unused-result -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration  -I./Include/internal  -I. -I./Include   -fPIC -DPy_BUILD_CORE -o Programs/python.o ./Programs/python.c
gcc -pthread -c -Wno-unused-result -Wsign-compare -DNDEBUG -g -fwrapv -O3 -Wall    -std=c99 -Wextra -Wno-unused-result -Wno-unused-parameter -Wno-missing-field-initializers -Werror=implicit-function-declaration  -I./Include/internal  -I. -I./Include   -fPIC -DPy_BUILD_CORE -o Parser/acceler.o Parser/acceler.c

===== TL;DR =====

real	0m28.704s
user	2m4.278s
sys	0m7.002s

(lfs chroot) root:/sources/Python-3.8.5# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/Python-3.8.5# time make install
if test "no-framework" = "no-framework" ; then \
	/usr/bin/install -c python /usr/bin/python3.8; \
else \
	/usr/bin/install -c -s Mac/pythonw /usr/bin/python3.8; \
fi

===== TL;DR =====

real	0m16.532s
user	0m21.582s
sys	0m4.845s

(lfs chroot) root:/sources/Python-3.8.5# 

```

<br>
### INPUT
```
chmod -v 755 /usr/lib/libpython3.8.so
chmod -v 755 /usr/lib/libpython3.so
ln -sfv pip3.8 /usr/bin/pip3

```

### OUTPUT
```
(lfs chroot) root:/sources/Python-3.8.5# chmod -v 755 /usr/lib/libpython3.8.so
mode of '/usr/lib/libpython3.8.so' retained as 0755 (rwxr-xr-x)

(lfs chroot) root:/sources/Python-3.8.5# chmod -v 755 /usr/lib/libpython3.so
mode of '/usr/lib/libpython3.so' retained as 0755 (rwxr-xr-x)

(lfs chroot) root:/sources/Python-3.8.5# ln -sfv pip3.8 /usr/bin/pip3
'/usr/bin/pip3' -> 'pip3.8'

(lfs chroot) root:/sources/Python-3.8.5# 

```

<br>
### INPUT
```
install -v -dm755 /usr/share/doc/python-3.8.5/html 

tar --strip-components=1  \
    --no-same-owner       \
    --no-same-permissions \
    -C /usr/share/doc/python-3.8.5/html \
    -xvf ../python-3.8.5-docs-html.tar.bz2

cd ../
rm -rf Python-3.8.5/

```

### OUTPUT
```
(lfs chroot) root:/sources/Python-3.8.5# install -v -dm755 /usr/share/doc/python-3.8.5/html 
install: creating directory '/usr/share/doc/python-3.8.5'
install: creating directory '/usr/share/doc/python-3.8.5/html'

(lfs chroot) root:/sources/Python-3.8.5# tar --strip-components=1  \
>     --no-same-owner       \
>     --no-same-permissions \
>     -C /usr/share/doc/python-3.8.5/html \

===== TL;DR =====

python-3.8.5-docs-html/faq/library.html
python-3.8.5-docs-html/faq/design.html
python-3.8.5-docs-html/genindex.html

(lfs chroot) root:/sources/Python-3.8.5# cd ../

(lfs chroot) root:/sources# rm -rf Python-3.8.5/

(lfs chroot) root:/sources# 

```

<br>
# Ninja-1.10.0

### INPUT
```
tar xf ninja-1.10.0.tar.gz
cd ninja-1.10.0/
export NINJAJOBS=4
sed -i '/int Guess/a \
  int   j = 0;\
  char* jobs = getenv( "NINJAJOBS" );\
  if ( jobs != NULL ) j = atoi( jobs );\
  if ( j > 0 ) return j;\
' src/ninja.cc

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf ninja-1.10.0.tar.gz

(lfs chroot) root:/sources# cd ninja-1.10.0/

(lfs chroot) root:/sources/ninja-1.10.0# export NINJAJOBS=4

(lfs chroot) root:/sources/ninja-1.10.0# sed -i '/int Guess/a \
>   int   j = 0;\
>   char* jobs = getenv( "NINJAJOBS" );\
>   if ( jobs != NULL ) j = atoi( jobs );\
>   if ( j > 0 ) return j;\
> ' src/ninja.cc

(lfs chroot) root:/sources/ninja-1.10.0# 

```

<br>
### INPUT
```
python3 configure.py --bootstrap

```

### OUTPUT
```
(lfs chroot) root:/sources/ninja-1.10.0# python3 configure.py --bootstrap
bootstrapping ninja...
warning: A compatible version of re2c (>= 0.11.3) was not found; changes to src/*.in.cc will not affect your build.
wrote build.ninja.
bootstrap complete.  rebuilding...
[29/29] LINK ninja

(lfs chroot) root:/sources/ninja-1.10.0# 

```

<br>
### INPUT
```
./ninja ninja_test
./ninja_test --gtest_filter=-SubprocessTest.SetWithLots

```

### OUTPUT
```
(lfs chroot) root:/sources/ninja-1.10.0# ./ninja ninja_test
[19/19] LINK ninja_test

(lfs chroot) root:/sources/ninja-1.10.0# ./ninja_test --gtest_filter=-SubprocessTest.SetWithLots
[341/341] ElideMiddle.ElideInTheMiddle
passed

(lfs chroot) root:/sources/ninja-1.10.0# 

```

<br>
### INPUT
```
install -vm755 ninja /usr/bin/
install -vDm644 misc/bash-completion /usr/share/bash-completion/completions/ninja
install -vDm644 misc/zsh-completion  /usr/share/zsh/site-functions/_ninja
cd ../
rm -rf ninja-1.10.0/

```

### OUTPUT
```
(lfs chroot) root:/sources/ninja-1.10.0# install -vm755 ninja /usr/bin/
'ninja' -> '/usr/bin/ninja'

(lfs chroot) root:/sources/ninja-1.10.0# install -vDm644 misc/bash-completion /usr/share/bash-completion/completions/ninja
'misc/bash-completion' -> '/usr/share/bash-completion/completions/ninja'

(lfs chroot) root:/sources/ninja-1.10.0# install -vDm644 misc/zsh-completion  /usr/share/zsh/site-functions/_ninja
install: creating directory '/usr/share/zsh'
install: creating directory '/usr/share/zsh/site-functions'
'misc/zsh-completion' -> '/usr/share/zsh/site-functions/_ninja'

(lfs chroot) root:/sources/ninja-1.10.0# cd ../

(lfs chroot) root:/sources# rm -rf ninja-1.10.0/

(lfs chroot) root:/sources# 

```

<br>
# Meson-0.55.0

### INPUT
```
tar xf meson-0.55.0.tar.gz
cd meson-0.55.0/

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf meson-0.55.0.tar.gz

(lfs chroot) root:/sources# cd meson-0.55.0/

(lfs chroot) root:/sources/meson-0.55.0# 

```

<br>
### INPUT
```
python3 setup.py build

```

### OUTPUT
```
(lfs chroot) root:/sources/meson-0.55.0# python3 setup.py build
running build
running build_py
creating build
creating build/lib

===== TL;DR =====

copying mesonbuild/dependencies/data/CMakeLists.txt -> build/lib/mesonbuild/dependencies/data
copying mesonbuild/dependencies/data/CMakeListsLLVM.txt -> build/lib/mesonbuild/dependencies/data
copying mesonbuild/dependencies/data/CMakePathInfo.txt -> build/lib/mesonbuild/dependencies/data

(lfs chroot) root:/sources/meson-0.55.0# 

```

<br>
### INPUT
```
python3 setup.py install --root=dest
cp -rv dest/* /

cd ../
rm -rf meson-0.55.0/

```

### OUTPUT
```
(lfs chroot) root:/sources/meson-0.55.0# python3 setup.py install --root=dest
running install
running build
running build_py

===== TL;DR =====

'dest/usr/share/polkit-1/actions' -> '/usr/share/polkit-1/actions'
'dest/usr/share/polkit-1/actions/com.mesonbuild.install.policy' -> '/usr/share/polkit-1/actions/com.mesonbuild.install.policy'
'dest/usr/bin/meson' -> '/usr/bin/meson'

(lfs chroot) root:/sources/meson-0.55.0# cd ../

(lfs chroot) root:/sources# rm -rf meson-0.55.0/

(lfs chroot) root:/sources# 

```

<br>
# Coreutils-8.32

### INPUT
```
tar xf coreutils-8.32.tar.xz
cd coreutils-8.32/
patch -Np1 -i ../coreutils-8.32-i18n-1.patch

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf coreutils-8.32.tar.xz

(lfs chroot) root:/sources# cd coreutils-8.32/

(lfs chroot) root:/sources/coreutils-8.32# patch -Np1 -i ../coreutils-8.32-i18n-1.patch
patching file bootstrap.conf
patching file configure.ac
patching file lib/linebuffer.h

===== TL;DR =====

patching file tests/misc/uniq.pl
patching file tests/pr/pr-tests.pl
patching file tests/unexpand/mb.sh

(lfs chroot) root:/sources/coreutils-8.32# 

```

<br>
### INPUT
```
sed -i '/test.lock/s/^/#/' gnulib-tests/gnulib.mk
autoreconf -fiv
FORCE_UNSAFE_CONFIGURE=1 ./configure \
            --prefix=/usr            \
            --enable-no-install-program=kill,uptime

```

### OUTPUT
```
(lfs chroot) root:/sources/coreutils-8.32# sed -i '/test.lock/s/^/#/' gnulib-tests/gnulib.mk

(lfs chroot) root:/sources/coreutils-8.32# autoreconf -fiv
autoreconf: Entering directory `.'
autoreconf: running: autopoint --force
Copying file build-aux/config.rpath

===== TL;DR =====

gnulib-tests/gnulib.mk:1359: library has 'test_rwlock1' as canonical name (possible typo)
gnulib-tests/Makefile.am:1:   'gnulib-tests/gnulib.mk' included from here
autoreconf: Leaving directory `.'

(lfs chroot) root:/sources/coreutils-8.32# FORCE_UNSAFE_CONFIGURE=1 ./configure \
>             --prefix=/usr            \
>             --enable-no-install-program=kill,uptime
checking for a BSD-compatible install... /usr/bin/install -c

===== TL;DR =====

config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

(lfs chroot) root:/sources/coreutils-8.32# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/coreutils-8.32# time make
  GEN      lib/alloca.h
  GEN      lib/configmake.h
  GEN      lib/arpa/inet.h

===== TL;DR =====

real	0m10.365s
user	0m48.792s
sys	0m6.118s

(lfs chroot) root:/sources/coreutils-8.32# 

```

<br>
### INPUT
```
time make NON_ROOT_USERNAME=tester check-root

```

### OUTPUT
```
(lfs chroot) root:/sources/coreutils-8.32# time make NON_ROOT_USERNAME=tester check-root
make check TESTS='tests/chown/basic.sh tests/cp/cp-a-selinux.sh tests/cp/preserve-gid.sh tests/cp/special-bits.sh tests/cp/cp-mv-enotsup-xattr.sh tests/cp/capability.sh tests/cp/sparse-fiemap.sh tests/cp/cross-dev-symlink.sh tests/dd/skip-seek-past-dev.sh tests/df/problematic-chars.sh tests/df/over-mount-device.sh tests/du/bind-mount-dir-cycle.sh ...

===== TL;DR =====

============================================================================
Testsuite summary for GNU coreutils 8.32
============================================================================
# TOTAL: 32
# PASS:  19
# SKIP:  13
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

real	0m1.263s
user	0m2.223s
sys	0m0.432s

(lfs chroot) root:/sources/coreutils-8.32# 

```

<br>
### INPUT
```
echo "dummy:x:102:tester" >> /etc/group
chown -Rv tester .

```

### OUTPUT
```
(lfs chroot) root:/sources/coreutils-8.32# echo "dummy:x:102:tester" >> /etc/group

(lfs chroot) root:/sources/coreutils-8.32# chown -Rv tester .
changed ownership of './bootstrap.conf' from root to tester
changed ownership of './autom4te.cache/requests' from root to tester
changed ownership of './autom4te.cache/traces.0' from root to tester

===== TL;DR =====

changed ownership of './src/tty.c' from root to tester
changed ownership of './src' from root to tester
changed ownership of '.' from root to tester

(lfs chroot) root:/sources/coreutils-8.32# 

```

<br>
### INPUT
```
su tester -c "PATH=$PATH make RUN_EXPENSIVE_TESTS=yes check"

```

### OUTPUT
```
(lfs chroot) root:/sources/coreutils-8.32# su tester -c "PATH=$PATH make RUN_EXPENSIVE_TESTS=yes check"
  GEN      public-submodule-commit
make  check-recursive
make[1]: Entering directory '/sources/coreutils-8.32'

===== TL;DR =====

============================================================================
Testsuite summary for GNU coreutils 8.32
============================================================================
# TOTAL: 625
# PASS:  493
# SKIP:  132
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU coreutils 8.32
============================================================================
# TOTAL: 345
# PASS:  327
# SKIP:  18
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

make[3]: Leaving directory '/sources/coreutils-8.32/gnulib-tests'
make[2]: Leaving directory '/sources/coreutils-8.32/gnulib-tests'
make[1]: Leaving directory '/sources/coreutils-8.32'

(lfs chroot) root:/sources/coreutils-8.32# 

```

<br>
### INPUT
```
sed -i '/dummy/d' /etc/group

```

### OUTPUT
```
(lfs chroot) root:/sources/coreutils-8.32# sed -i '/dummy/d' /etc/group

(lfs chroot) root:/sources/coreutils-8.32# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/coreutils-8.32# time make install
make  install-recursive
make[1]: Entering directory '/sources/coreutils-8.32'
Making install in po
make[2]: Entering directory '/sources/coreutils-8.32/po'

===== TL;DR =====

real	0m1.935s
user	0m1.427s
sys	0m0.490s

(lfs chroot) root:/sources/coreutils-8.32# 

```

<br>
### INPUT
```
mv -v /usr/bin/{cat,chgrp,chmod,chown,cp,date,dd,df,echo} /bin
mv -v /usr/bin/{false,ln,ls,mkdir,mknod,mv,pwd,rm} /bin
mv -v /usr/bin/{rmdir,stty,sync,true,uname} /bin
mv -v /usr/bin/chroot /usr/sbin
mv -v /usr/share/man/man1/chroot.1 /usr/share/man/man8/chroot.8
sed -i 's/"1"/"8"/' /usr/share/man/man8/chroot.8
mv -v /usr/bin/{head,nice,sleep,touch} /bin
cd ../
rm -rf coreutils-8.32/

```

### OUTPUT
```
(lfs chroot) root:/sources/coreutils-8.32# mv -v /usr/bin/{cat,chgrp,chmod,chown,cp,date,dd,df,echo} /bin
renamed '/usr/bin/cat' -> '/bin/cat'
renamed '/usr/bin/chgrp' -> '/bin/chgrp'
renamed '/usr/bin/chmod' -> '/bin/chmod'
renamed '/usr/bin/chown' -> '/bin/chown'
renamed '/usr/bin/cp' -> '/bin/cp'
renamed '/usr/bin/date' -> '/bin/date'
renamed '/usr/bin/dd' -> '/bin/dd'
renamed '/usr/bin/df' -> '/bin/df'
renamed '/usr/bin/echo' -> '/bin/echo'

(lfs chroot) root:/sources/coreutils-8.32# mv -v /usr/bin/{false,ln,ls,mkdir,mknod,mv,pwd,rm} /bin
renamed '/usr/bin/false' -> '/bin/false'
renamed '/usr/bin/ln' -> '/bin/ln'
renamed '/usr/bin/ls' -> '/bin/ls'
renamed '/usr/bin/mkdir' -> '/bin/mkdir'
renamed '/usr/bin/mknod' -> '/bin/mknod'
renamed '/usr/bin/mv' -> '/bin/mv'
renamed '/usr/bin/pwd' -> '/bin/pwd'
renamed '/usr/bin/rm' -> '/bin/rm'

(lfs chroot) root:/sources/coreutils-8.32# mv -v /usr/bin/{rmdir,stty,sync,true,uname} /bin
renamed '/usr/bin/rmdir' -> '/bin/rmdir'
renamed '/usr/bin/stty' -> '/bin/stty'
renamed '/usr/bin/sync' -> '/bin/sync'
renamed '/usr/bin/true' -> '/bin/true'
renamed '/usr/bin/uname' -> '/bin/uname'

(lfs chroot) root:/sources/coreutils-8.32# mv -v /usr/bin/chroot /usr/sbin
renamed '/usr/bin/chroot' -> '/usr/sbin/chroot'

(lfs chroot) root:/sources/coreutils-8.32# mv -v /usr/share/man/man1/chroot.1 /usr/share/man/man8/chroot.8
renamed '/usr/share/man/man1/chroot.1' -> '/usr/share/man/man8/chroot.8'

(lfs chroot) root:/sources/coreutils-8.32# sed -i 's/"1"/"8"/' /usr/share/man/man8/chroot.8

(lfs chroot) root:/sources/coreutils-8.32# mv -v /usr/bin/{head,nice,sleep,touch} /bin
renamed '/usr/bin/head' -> '/bin/head'
renamed '/usr/bin/nice' -> '/bin/nice'
renamed '/usr/bin/sleep' -> '/bin/sleep'
renamed '/usr/bin/touch' -> '/bin/touch'

(lfs chroot) root:/sources/coreutils-8.32# cd ../

(lfs chroot) root:/sources# rm -rf coreutils-8.32/

(lfs chroot) root:/sources# 

```

<br>
# Check-0.15.2

### INPUT
```
tar xf check-0.15.2.tar.gz
cd check-0.15.2/
./configure --prefix=/usr --disable-static

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf check-0.15.2.tar.gz

(lfs chroot) root:/sources# cd check-0.15.2/

(lfs chroot) root:/sources/check-0.15.2# ./configure --prefix=/usr --disable-static
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

==========================================
Summary of Check 0.15.2 options:

fork mode ............................ yes
high resolution timer replacement .... no
snprintf replacement ................. no
subunit support....................... no
timeout unit tests ................... yes
POSIX regular expressions ............ yes
build docs ........................... yes
==========================================

(lfs chroot) root:/sources/check-0.15.2# 

```

<br>
### INPUT
```
time make


```

### OUTPUT
```
(lfs chroot) root:/sources/check-0.15.2# time make
make  all-recursive
make[1]: Entering directory '/sources/check-0.15.2'
Making all in lib

===== TL;DR =====

real	0m2.114s
user	0m7.714s
sys	0m0.824s

(lfs chroot) root:/sources/check-0.15.2# 

```


<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/check-0.15.2# time make check
Making check in lib
make[1]: Entering directory '/sources/check-0.15.2/lib'
make[1]: Nothing to be done for 'check'.

===== TL;DR =====

============================================================================
Testsuite summary for Check 0.15.2
============================================================================
# TOTAL: 1
# PASS:  1
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for Check 0.15.2
============================================================================
# TOTAL: 9
# PASS:  9
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

real	2m56.601s
user	0m2.217s
sys	0m0.835s

(lfs chroot) root:/sources/check-0.15.2# 

```

<br>
### INPUT
```
time make docdir=/usr/share/doc/check-0.15.2 install

```

### OUTPUT
```
(lfs chroot) root:/sources/check-0.15.2# time make docdir=/usr/share/doc/check-0.15.2 install
Making install in lib
make[1]: Entering directory '/sources/check-0.15.2/lib'
make[2]: Entering directory '/sources/check-0.15.2/lib'
make[2]: Nothing to be done for 'install-exec-am'.

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

real	0m0.348s
user	0m0.214s
sys	0m0.110s

(lfs chroot) root:/sources/check-0.15.2#

```

<br>
### INPUT
```
cd ../
rm -rf check-0.15.2/

```

### OUTPUT
```
(lfs chroot) root:/sources/check-0.15.2# cd ../

(lfs chroot) root:/sources# rm -rf check-0.15.2/

(lfs chroot) root:/sources# 

```

<br>
# Diffutils-3.7

### INPUT
```
tar xf diffutils-3.7.tar.xz
cd diffutils-3.7/
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf diffutils-3.7.tar.xz

(lfs chroot) root:/sources# cd diffutils-3.7/

(lfs chroot) root:/sources/diffutils-3.7# ./configure --prefix=/usr
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

(lfs chroot) root:/sources/diffutils-3.7# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/diffutils-3.7# time make
Making all in lib
make[1]: Entering directory '/sources/diffutils-3.7/lib'
  GEN      alloca.h
  GEN      ctype.h

===== TL;DR =====

real	0m3.588s
user	0m8.872s
sys	0m1.045s

(lfs chroot) root:/sources/diffutils-3.7# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/diffutils-3.7# time make check
  GEN      public-submodule-commit
Making check in lib
make[1]: Entering directory '/sources/diffutils-3.7/lib'
make  check-am

===== TL;DR =====

============================================================================
Testsuite summary for GNU diffutils 3.7
============================================================================
# TOTAL: 22
# PASS:  20
# SKIP:  1
# XFAIL: 1
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU diffutils 3.7
============================================================================
# TOTAL: 173
# PASS:  160
# SKIP:  13
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

real	0m5.562s
user	0m15.161s
sys	0m2.665s

(lfs chroot) root:/sources/diffutils-3.7# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/diffutils-3.7# time make install
Making install in lib
make[1]: Entering directory '/sources/diffutils-3.7/lib'
make  install-am

===== TL;DR =====

real	0m1.178s
user	0m0.677s
sys	0m0.259s

(lfs chroot) root:/sources/diffutils-3.7#

```

<br>
### INPUT
```
cd ../
rm -rf diffutils-3.7/

```

### OUTPUT
```
(lfs chroot) root:/sources/diffutils-3.7# cd ../

(lfs chroot) root:/sources# rm -rf diffutils-3.7/

(lfs chroot) root:/sources# 

```

<br>
# Gawk-5.1.0

### INPUT
```
tar xf gawk-5.1.0.tar.xz
cd gawk-5.1.0/
sed -i 's/extras//' Makefile.in
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf gawk-5.1.0.tar.xz

(lfs chroot) root:/sources# cd gawk-5.1.0/

(lfs chroot) root:/sources/gawk-5.1.0# sed -i 's/extras//' Makefile.in

(lfs chroot) root:/sources/gawk-5.1.0# ./configure --prefix=/usr
checking for a BSD-compatible install... ./install-sh -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p.

===== TL;DR =====

config.status: creating po/POTFILES
config.status: creating po/Makefile
config.status: executing libtool commands

(lfs chroot) root:/sources/gawk-5.1.0# 

```

<br>
### INPUT
```
time make 

```

### OUTPUT
```
(lfs chroot) root:/sources/gawk-5.1.0# time make 
make  all-recursive
make[1]: Entering directory '/sources/gawk-5.1.0'
Making all in support

===== TL;DR =====

real	0m6.420s
user	0m24.309s
sys	0m1.609s

(lfs chroot) root:/sources/gawk-5.1.0# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/gawk-5.1.0# time make check
make  check-recursive
make[1]: Entering directory '/sources/gawk-5.1.0'
Making check in support

===== TL;DR =====

======== Starting basic tests ========
======== Done with basic tests ========

======== Starting Unix tests ========
======== Done with Unix tests ========

======== Starting gawk extension tests ========
======== Done with gawk extension tests ========

======== Starting machine-specific tests ========
======== Done with machine-specific tests ========

======== Starting shared library tests ========
======== Done with shared library tests ========

======== Starting MPFR tests ========
======== Done with MPFR tests ========

======== Starting tests that can vary based on character set or locale support ========
**************************************************************************
* Some or all of these tests may fail if you have inadequate or missing  *
* locale support. At least en_US.UTF-8, fr_FR.UTF-8, ru_RU.UTF-8 and     *
fnmatch
fork
* ja_JP.UTF-8 are needed. The el_GR.iso88597 is optional but helpful.    *
* However, if you see this message, the Makefile thinks you have what    *
* you need ...                                                           *
fork2
**************************************************************************
======== Done with tests that can vary based on character set or locale support ========

===== TL;DR =====

real	0m4.966s
user	0m5.403s
sys	0m0.780s

(lfs chroot) root:/sources/gawk-5.1.0# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/gawk-5.1.0# time make install
make  install-recursive
make[1]: Entering directory '/sources/gawk-5.1.0'
Making install in support

===== TL;DR =====

----------------------------------------------------------------------
Libraries have been installed in:
   /usr/lib/gawk

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

real	0m0.924s
user	0m0.688s
sys	0m0.088s

(lfs chroot) root:/sources/gawk-5.1.0# 

```

<br>
### INPUT
```
mkdir -v /usr/share/doc/gawk-5.1.0
cp    -v doc/{awkforai.txt,*.{eps,pdf,jpg}} /usr/share/doc/gawk-5.1.0
cd ../
rm -rf gawk-5.1.0/

```

### OUTPUT
```
(lfs chroot) root:/sources/gawk-5.1.0# mkdir -v /usr/share/doc/gawk-5.1.0
mkdir: created directory '/usr/share/doc/gawk-5.1.0'

(lfs chroot) root:/sources/gawk-5.1.0# cp    -v doc/{awkforai.txt,*.{eps,pdf,jpg}} /usr/share/doc/gawk-5.1.0
'doc/awkforai.txt' -> '/usr/share/doc/gawk-5.1.0/awkforai.txt'
'doc/api-figure1.eps' -> '/usr/share/doc/gawk-5.1.0/api-figure1.eps'
'doc/api-figure2.eps' -> '/usr/share/doc/gawk-5.1.0/api-figure2.eps'

===== TL;DR =====

'doc/rflashlight.pdf' -> '/usr/share/doc/gawk-5.1.0/rflashlight.pdf'
'doc/statist.pdf' -> '/usr/share/doc/gawk-5.1.0/statist.pdf'
'doc/statist.jpg' -> '/usr/share/doc/gawk-5.1.0/statist.jpg'

(lfs chroot) root:/sources/gawk-5.1.0# cd ../

(lfs chroot) root:/sources# rm -rf gawk-5.1.0/

(lfs chroot) root:/sources# 

```

<br>
# Findutils-4.7.0

### INPUT
```
tar xf findutils-4.7.0.tar.xz
cd findutils-4.7.0/
./configure --prefix=/usr --localstatedir=/var/lib/locate

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf findutils-4.7.0.tar.xz

(lfs chroot) root:/sources# cd findutils-4.7.0/

(lfs chroot) root:/sources/findutils-4.7.0# ./configure --prefix=/usr --localstatedir=/var/lib/locate
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

(lfs chroot) root:/sources/findutils-4.7.0# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/findutils-4.7.0# time make
make  all-recursive
make[1]: Entering directory '/sources/findutils-4.7.0'
Making all in gl

===== TL;DR =====

real	0m4.882s
user	0m11.903s
sys	0m1.569s

(lfs chroot) root:/sources/findutils-4.7.0#

```

<br>
### INPUT
```
chown -Rv tester .
su tester -c "PATH=$PATH make check"

```

### OUTPUT
```
(lfs chroot) root:/sources/findutils-4.7.0# chown -Rv tester .
changed ownership of './Makefile.in' from root to tester
changed ownership of './README-hacking' from root to tester
changed ownership of './ChangeLog' from root to tester

===== TL;DR =====

changed ownership of './po/nl.gmo' from root to tester
changed ownership of './po' from root to tester
changed ownership of '.' from root to tester

(lfs chroot) root:/sources/findutils-4.7.0# su tester -c "PATH=$PATH make check"
  GEN      public-submodule-commit
Making check in gl
make[1]: Entering directory '/sources/findutils-4.7.0/gl'

===== TL;DR =====

============================================================================
Testsuite summary for GNU findutils 4.7.0
============================================================================
# TOTAL: 2
# PASS:  2
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU findutils 4.7.0
============================================================================
# TOTAL: 236
# PASS:  224
# SKIP:  12
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU findutils 4.7.0
============================================================================
# TOTAL: 12
# PASS:  11
# SKIP:  1
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

make[3]: Leaving directory '/sources/findutils-4.7.0'
make[2]: Leaving directory '/sources/findutils-4.7.0'
make[1]: Leaving directory '/sources/findutils-4.7.0'

(lfs chroot) root:/sources/findutils-4.7.0# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/findutils-4.7.0# time make install
Making install in gl
make[1]: Entering directory '/sources/findutils-4.7.0/gl'
Making install in lib

===== TL;DR =====

real	0m1.139s
user	0m0.943s
sys	0m0.228s

(lfs chroot) root:/sources/findutils-4.7.0# 

```

<br>
### INPUT
```
mv -v /usr/bin/find /bin
sed -i 's|find:=${BINDIR}|find:=/bin|' /usr/bin/updatedb
cd ../
rm -rf findutils-4.7.0/

```

### OUTPUT
```
(lfs chroot) root:/sources/findutils-4.7.0# mv -v /usr/bin/find /bin
renamed '/usr/bin/find' -> '/bin/find'

(lfs chroot) root:/sources/findutils-4.7.0# sed -i 's|find:=${BINDIR}|find:=/bin|' /usr/bin/updatedb

(lfs chroot) root:/sources/findutils-4.7.0# cd ../

(lfs chroot) root:/sources# rm -rf findutils-4.7.0/

(lfs chroot) root:/sources# 

```

<br>
# Groff-1.22.4

### INPUT
```
tar xf groff-1.22.4.tar.gz
cd groff-1.22.4/
PAGE=A4 ./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf groff-1.22.4.tar.gz

(lfs chroot) root:/sources# cd groff-1.22.4/

(lfs chroot) root:/sources/groff-1.22.4# PAGE=A4 ./configure --prefix=/usr
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out

===== TL;DR =====

GNU Troff version 1.22.4
----------------------------------------------------------------------
 Prefix                          : /usr
 Compiler                        : gcc -g -O2 
 X11 support                     : no
 Doc build                       : yes 
 URW fonts for pdf               : no
 Use uchardet library for preconv: no
 pdftools for distcheck          : no
----------------------------------------------------------------------

(lfs chroot) root:/sources/groff-1.22.4#

```

<br>
### INPUT
```
time make -j1

```

### OUTPUT
```
(lfs chroot) root:/sources/groff-1.22.4# time make -j1
  GEN      lib/alloca.h
  GEN      lib/limits.h
  GEN      lib/math.h

===== TL;DR =====

real	0m43.727s
user	0m41.253s
sys	0m2.518s

(lfs chroot) root:/sources/groff-1.22.4# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/groff-1.22.4# time make install
make  install-am
make[1]: Entering directory '/sources/groff-1.22.4'
make[2]: Entering directory '/sources/groff-1.22.4'

===== TL;DR =====

real	0m0.376s
user	0m0.714s
sys	0m0.134s

(lfs chroot) root:/sources/groff-1.22.4# 

```

<br>
### INPUT
```
cd ../
rm -rf groff-1.22.4/

```

### OUTPUT
```
(lfs chroot) root:/sources/groff-1.22.4# cd ../

(lfs chroot) root:/sources# rm -rf groff-1.22.4/

(lfs chroot) root:/sources# 

```

<br>
# GRUB-2.04

### INPUT
```
tar xf grub-2.04.tar.xz
cd grub-2.04/
./configure --prefix=/usr          \
            --sbindir=/sbin        \
            --sysconfdir=/etc      \
            --disable-efiemu       \
            --disable-werror

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf grub-2.04.tar.xz

(lfs chroot) root:/sources# cd grub-2.04/

(lfs chroot) root:/sources/grub-2.04# ./configure --prefix=/usr          \
>             --sbindir=/sbin        \
>             --sysconfdir=/etc      \
>             --disable-efiemu       \

===== TL;DR =====

*******************************************************
GRUB2 will be compiled with following components:
Platform: i386-pc
With devmapper support: No (need libdevmapper header)
With memory debugging: No
With disk cache statistics: No
With boot time statistics: No
efiemu runtime: No (explicitly disabled)
grub-mkfont: No (need freetype2 library)
grub-mount: No (need FUSE library)
starfield theme: No (No build-time grub-mkfont)
With libzfs support: No (need zfs library)
Build-time grub-mkfont: No (need freetype2 library)
Without unifont (no build-time grub-mkfont)
With liblzma from -llzma (support for XZ-compressed mips images)
*******************************************************

(lfs chroot) root:/sources/grub-2.04# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/grub-2.04# time make
bison -d -p grub_script_yy -b grub_script ./grub-core/script/parser.y
flex -o grub_script.yy.c --header-file=grub_script.yy.h ./grub-core/script/yylex.l

===== TL;DR =====

real	0m21.226s
user	1m17.096s
sys	0m14.606s

(lfs chroot) root:/sources/grub-2.04# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/grub-2.04# time make install
make  install-recursive
make[1]: Entering directory '/sources/grub-2.04'
Making install in grub-core/lib/gnulib

===== TL;DR =====

real	0m1.193s
user	0m1.061s
sys	0m0.188s

(lfs chroot) root:/sources/grub-2.04#

```

<br>
### INPUT
```
mv -v /etc/bash_completion.d/grub /usr/share/bash-completion/completions
cd ../
rm -rf grub-2.04/

```

### OUTPUT
```
(lfs chroot) root:/sources/grub-2.04# mv -v /etc/bash_completion.d/grub /usr/share/bash-completion/completions
renamed '/etc/bash_completion.d/grub' -> '/usr/share/bash-completion/completions/grub'

(lfs chroot) root:/sources/grub-2.04# cd ../

(lfs chroot) root:/sources# rm -rf grub-2.04/

(lfs chroot) root:/sources# 

```

<br>
# Less-551

### INPUT
```
tar xf less-551.tar.gz
cd less-551/
./configure --prefix=/usr --sysconfdir=/etc

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf less-551.tar.gz

(lfs chroot) root:/sources# cd less-551/

(lfs chroot) root:/sources/less-551# ./configure --prefix=/usr --sysconfdir=/etc
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out

===== TL;DR =====

configure: creating ./config.status
config.status: creating Makefile
config.status: creating defines.h

(lfs chroot) root:/sources/less-551# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/less-551# time make
test ! -f stamp-h || CONFIG_FILES= CONFIG_HEADERS=defines.h ./config.status
touch stamp-h
gcc -I. -c -DBINDIR=\"/usr/bin\" -DSYSDIR=\"/etc\"  -g -O2 lesskey.c

===== TL;DR =====

real	0m0.887s
user	0m3.909s
sys	0m0.293s

(lfs chroot) root:/sources/less-551# 

```

<br>
### INPUT
```
time make install
cd ../
rm -rf less-551/

```

### OUTPUT
```
(lfs chroot) root:/sources/less-551# time make install
./mkinstalldirs /usr/bin /usr/share/man/man1
/usr/bin/install -c less /usr/bin/less
/usr/bin/install -c lesskey /usr/bin/lesskey
/usr/bin/install -c lessecho /usr/bin/lessecho
/usr/bin/install -c -m 644 ./less.nro /usr/share/man/man1/less.1
/usr/bin/install -c -m 644 ./lesskey.nro /usr/share/man/man1/lesskey.1
/usr/bin/install -c -m 644 ./lessecho.nro /usr/share/man/man1/lessecho.1

real	0m0.086s
user	0m0.033s
sys	0m0.019s

(lfs chroot) root:/sources/less-551# cd ../

(lfs chroot) root:/sources# rm -rf less-551/

(lfs chroot) root:/sources# 

```

<br>
# Gzip-1.10

### INPUT
```
tar xf gzip-1.10.tar.xz
cd gzip-1.10/
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf gzip-1.10.tar.xz

(lfs chroot) root:/sources# cd gzip-1.10/

(lfs chroot) root:/sources/gzip-1.10# ./configure --prefix=/usr
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: creating tests/Makefile
config.status: creating lib/config.h
config.status: executing depfiles commands

(lfs chroot) root:/sources/gzip-1.10# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/gzip-1.10# time make
  GEN      version.c
  GEN      version.h
make  all-recursive

===== TL;DR =====

real	0m1.395s
user	0m3.804s
sys	0m0.751s

(lfs chroot) root:/sources/gzip-1.10# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/gzip-1.10# time make check
  GEN      public-submodule-commit
make  check-recursive
make[1]: Entering directory '/sources/gzip-1.10'

===== TL;DR =====

============================================================================
Testsuite summary for gzip 1.10
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

real	0m1.796s
user	0m1.941s
sys	0m0.444s

(lfs chroot) root:/sources/gzip-1.10# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/gzip-1.10# time make install
make  install-recursive
make[1]: Entering directory '/sources/gzip-1.10'
Making install in lib

===== TL;DR =====

real	0m0.493s
user	0m0.415s
sys	0m0.063s

(lfs chroot) root:/sources/gzip-1.10# 

```

<br>
### INPUT
```
mv -v /usr/bin/gzip /bin
cd ../
rm -rf gzip-1.10/

```

### OUTPUT
```
(lfs chroot) root:/sources/gzip-1.10# mv -v /usr/bin/gzip /bin
renamed '/usr/bin/gzip' -> '/bin/gzip'

(lfs chroot) root:/sources/gzip-1.10# cd ../

(lfs chroot) root:/sources# rm -rf gzip-1.10/

(lfs chroot) root:/sources# 

```

<br>
# IPRoute2-5.8.0

### INPUT
```
tar xf iproute2-5.8.0.tar.xz
cd iproute2-5.8.0/
sed -i /ARPD/d Makefile
rm -fv man/man8/arpd.8
sed -i 's/.m_ipt.o//' tc/Makefile

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf iproute2-5.8.0.tar.xz

(lfs chroot) root:/sources# cd iproute2-5.8.0/

(lfs chroot) root:/sources/iproute2-5.8.0# sed -i /ARPD/d Makefile

(lfs chroot) root:/sources/iproute2-5.8.0# rm -fv man/man8/arpd.8
removed 'man/man8/arpd.8'

(lfs chroot) root:/sources/iproute2-5.8.0# sed -i 's/.m_ipt.o//' tc/Makefile

(lfs chroot) root:/sources/iproute2-5.8.0# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/iproute2-5.8.0# time make
sh configure /usr/include
TC schedulers
 ATM	no

===== TL;DR =====

real	0m5.920s
user	0m22.638s
sys	0m2.404s

(lfs chroot) root:/sources/iproute2-5.8.0#

```

<br>
### INPUT
```
make DOCDIR=/usr/share/doc/iproute2-5.8.0 install

```

### OUTPUT
```
(lfs chroot) root:/sources/iproute2-5.8.0# make DOCDIR=/usr/share/doc/iproute2-5.8.0 install

lib
make[1]: warning: -j6 forced in submake: resetting jobserver mode.
make[1]: Nothing to be done for 'all'.

===== TL;DR =====

install -m 0644 bash-completion/tc /usr/share/bash-completion/completions
install -m 0644 bash-completion/devlink /usr/share/bash-completion/completions
install -m 0644 include/bpf_elf.h /usr/include/iproute2

(lfs chroot) root:/sources/iproute2-5.8.0# 

```

<br>
### INPUT
```
cd ../
rm -rf iproute2-5.8.0/

```

### OUTPUT
```
(lfs chroot) root:/sources/iproute2-5.8.0# cd ../

(lfs chroot) root:/sources# rm -rf iproute2-5.8.0/

(lfs chroot) root:/sources# 

```

<br>
# Kbd-2.3.0

### INPUT
```
tar xf kbd-2.3.0.tar.xz
cd kbd-2.3.0/
patch -Np1 -i ../kbd-2.3.0-backspace-1.patch

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf kbd-2.3.0.tar.xz

(lfs chroot) root:/sources# cd kbd-2.3.0/

(lfs chroot) root:/sources/kbd-2.3.0# patch -Np1 -i ../kbd-2.3.0-backspace-1.patch
patching file data/keymaps/i386/dvorak/dvorak-l.map
patching file data/keymaps/i386/dvorak/dvorak-r.map
patching file data/keymaps/i386/fgGIod/tr_f-latin5.map
patching file data/keymaps/i386/qwerty/lt.l4.map
patching file data/keymaps/i386/qwerty/lt.map
patching file data/keymaps/i386/qwerty/no-latin1.map
patching file data/keymaps/i386/qwerty/ru1.map
patching file data/keymaps/i386/qwerty/ru2.map
patching file data/keymaps/i386/qwerty/ru-cp1251.map
patching file data/keymaps/i386/qwerty/ru-ms.map
patching file data/keymaps/i386/qwerty/ru_win.map
patching file data/keymaps/i386/qwerty/se-ir209.map
patching file data/keymaps/i386/qwerty/se-lat6.map
patching file data/keymaps/i386/qwerty/tr_q-latin5.map
patching file data/keymaps/i386/qwerty/ua.map
patching file data/keymaps/i386/qwerty/ua-utf.map
patching file data/keymaps/i386/qwerty/ua-utf-ws.map
patching file data/keymaps/i386/qwerty/ua-ws.map

(lfs chroot) root:/sources/kbd-2.3.0# 

```

<br>
### INPUT
```
sed -i '/RESIZECONS_PROGS=/s/yes/no/' configure
sed -i 's/resizecons.8 //' docs/man/man8/Makefile.in
./configure --prefix=/usr --disable-vlock

```

### OUTPUT
```
(lfs chroot) root:/sources/kbd-2.3.0# sed -i '/RESIZECONS_PROGS=/s/yes/no/' configure

(lfs chroot) root:/sources/kbd-2.3.0# sed -i 's/resizecons.8 //' docs/man/man8/Makefile.in

(lfs chroot) root:/sources/kbd-2.3.0# ./configure --prefix=/usr --disable-vlock
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

	kbd 2.3.0
	======

	prefix:                 /usr
	libdir:                 ${exec_prefix}/lib
	bindir:                 ${exec_prefix}/bin
	datadir:                ${datarootdir}

	compiler:               gcc
	cflags:                 -O2 -D_FORTIFY_SOURCE=2  -Waggregate-return -Wall -Wcast-align -Wconversion -Wdisabled-optimization -Wextra -Wmissing-declarations -Wmissing-format-attribute -Wmissing-noreturn -Wmissing-prototypes -Wpointer-arith -Wredundant-decls -Wshadow -Wstrict-prototypes -Wwrite-strings

	{get,set}keycodes:      yes
	resizecons:             no
	optional progs:         no
	vlock:                  no
	libkbdfile:             no
	libkeymap:              no

(lfs chroot) root:/sources/kbd-2.3.0# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/kbd-2.3.0# time make
make  all-recursive
make[1]: Entering directory '/sources/kbd-2.3.0'
Making all in src

===== TL;DR =====

real	0m4.767s
user	0m14.309s
sys	0m2.711s

(lfs chroot) root:/sources/kbd-2.3.0#

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/kbd-2.3.0# time make check
Making check in src
make[1]: Entering directory '/sources/kbd-2.3.0/src'
Making check in libcommon

===== TL;DR =====

## ------------- ##
## Test results. ##
## ------------- ##

36 tests were successful.
4 tests were skipped.

===== TL;DR =====

real	0m0.717s
user	0m0.642s
sys	0m0.095s

(lfs chroot) root:/sources/kbd-2.3.0# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/kbd-2.3.0# time make install
Making install in src
make[1]: Entering directory '/sources/kbd-2.3.0/src'
Making install in libcommon

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

real	0m1.216s
user	0m0.849s
sys	0m0.165s

(lfs chroot) root:/sources/kbd-2.3.0# 

```

<br>
### INPUT
```
rm -v /usr/lib/libtswrap.{a,la,so*}
mkdir -v            /usr/share/doc/kbd-2.3.0
cp -R -v docs/doc/* /usr/share/doc/kbd-2.3.0

```

### OUTPUT
```
(lfs chroot) root:/sources/kbd-2.3.0# rm -v /usr/lib/libtswrap.{a,la,so*}
removed '/usr/lib/libtswrap.a'
removed '/usr/lib/libtswrap.la'
removed '/usr/lib/libtswrap.so'
removed '/usr/lib/libtswrap.so.1'
removed '/usr/lib/libtswrap.so.1.0.0'

(lfs chroot) root:/sources/kbd-2.3.0# mkdir -v            /usr/share/doc/kbd-2.3.0
mkdir: created directory '/usr/share/doc/kbd-2.3.0'

(lfs chroot) root:/sources/kbd-2.3.0# cp -R -v docs/doc/* /usr/share/doc/kbd-2.3.0
'docs/doc/A20' -> '/usr/share/doc/kbd-2.3.0/A20'
'docs/doc/A20/A20.html' -> '/usr/share/doc/kbd-2.3.0/A20/A20.html'
'docs/doc/A20/xfix-286mode2' -> '/usr/share/doc/kbd-2.3.0/A20/xfix-286mode2'

===== TL;DR =====

'docs/doc/utf/'$'\342\231\252\342\231\254' -> '/usr/share/doc/kbd-2.3.0/utf/'$'\342\231\252\342\231\254'
'docs/doc/utf/utfdemo' -> '/usr/share/doc/kbd-2.3.0/utf/utfdemo'
'docs/doc/utf/README' -> '/usr/share/doc/kbd-2.3.0/utf/README'

(lfs chroot) root:/sources/kbd-2.3.0# 

```

<br>
### INPUT
```
cd ../
rm -rf kbd-2.3.0/

```

### OUTPUT
```
(lfs chroot) root:/sources/kbd-2.3.0# cd ../

(lfs chroot) root:/sources# rm -rf kbd-2.3.0/

(lfs chroot) root:/sources# 

```

<br>
# Libpipeline-1.5.3

### INPUT
```
tar xf libpipeline-1.5.3.tar.gz
cd libpipeline-1.5.3/
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf libpipeline-1.5.3.tar.gz

(lfs chroot) root:/sources# cd libpipeline-1.5.3/

(lfs chroot) root:/sources/libpipeline-1.5.3# ./configure --prefix=/usr
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: creating config.h
config.status: executing depfiles commands
config.status: executing libtool commands

(lfs chroot) root:/sources/libpipeline-1.5.3# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/libpipeline-1.5.3# time make
make  all-recursive
make[1]: Entering directory '/sources/libpipeline-1.5.3'
Making all in gl/lib

===== TL;DR =====

real	0m1.627s
user	0m3.034s
sys	0m0.544s

(lfs chroot) root:/sources/libpipeline-1.5.3# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/libpipeline-1.5.3# time make check
Making check in gl/lib
make[1]: Entering directory '/sources/libpipeline-1.5.3/gl/lib'
make  check-recursive

===== TL;DR =====

============================================================================
Testsuite summary for libpipeline 1.5.3
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

real	0m1.094s
user	0m2.667s
sys	0m0.355s

(lfs chroot) root:/sources/libpipeline-1.5.3# 

```

<br>
### INPUT
```
time make install
cd ../
rm -rf libpipeline-1.5.3/

```

### OUTPUT
```
(lfs chroot) root:/sources/libpipeline-1.5.3# time make install
Making install in gl/lib
make[1]: Entering directory '/sources/libpipeline-1.5.3/gl/lib'
make  install-recursive

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

real	0m0.934s
user	0m0.372s
sys	0m0.209s

(lfs chroot) root:/sources/libpipeline-1.5.3# cd ../

(lfs chroot) root:/sources# rm -rf libpipeline-1.5.3/

(lfs chroot) root:/sources# 

```

<br>
# Make-4.3

### INPUT
```
tar xf make-4.3.tar.gz
cd make-4.3/
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf make-4.3.tar.gz

(lfs chroot) root:/sources# cd make-4.3/

(lfs chroot) root:/sources/make-4.3# ./configure --prefix=/usr
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

(lfs chroot) root:/sources/make-4.3# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/make-4.3# time make
Making all in lib
make[1]: Entering directory '/sources/make-4.3/lib'
rm -f alloca.h-t alloca.h && \

===== TL;DR =====

real	0m1.448s
user	0m5.717s
sys	0m0.681s

(lfs chroot) root:/sources/make-4.3# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/make-4.3# time make check
Making check in lib
make[1]: Entering directory '/sources/make-4.3/lib'
make  check-recursive

===== TL;DR =====

------------------------------------------------------------------------------
        Running tests for GNU make on Linux osp 4.19.0-13-amd64 x86_64
                                 GNU Make 4.3
------------------------------------------------------------------------------

===== TL;DR =====

690 Tests in 125 Categories Complete ... No Failures :-)

======================================================================
 Regression PASSED: GNU Make 4.3 (x86_64-pc-linux-gnu) built with gcc 
======================================================================

real	0m45.398s
user	0m3.671s
sys	0m2.541s

(lfs chroot) root:/sources/make-4.3# 

```

<br>
### INPUT
```
time make install
cd ../
rm -rf make-4.3/

```

### OUTPUT
```
(lfs chroot) root:/sources/make-4.3# time make install
Making install in lib
make[1]: Entering directory '/sources/make-4.3/lib'
make  install-recursive

===== TL;DR =====

real	0m0.702s
user	0m0.399s
sys	0m0.216s

(lfs chroot) root:/sources/make-4.3# cd ../

(lfs chroot) root:/sources# rm -rf make-4.3/

(lfs chroot) root:/sources# 

```

<br>
# Patch-2.7.6

### INPUT
```
tar xf patch-2.7.6.tar.xz
cd patch-2.7.6/
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf patch-2.7.6.tar.xz

(lfs chroot) root:/sources# cd patch-2.7.6/

(lfs chroot) root:/sources/patch-2.7.6# ./configure --prefix=/usr
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p

===== TL;DR =====

config.status: creating tests/Makefile
config.status: creating config.h
config.status: executing depfiles commands

(lfs chroot) root:/sources/patch-2.7.6# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/patch-2.7.6# time make
make  all-recursive
make[1]: Entering directory '/sources/patch-2.7.6'
Making all in lib

===== TL;DR =====

real	0m2.040s
user	0m7.345s
sys	0m1.167s

(lfs chroot) root:/sources/patch-2.7.6# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/patch-2.7.6# time make check
  GEN      public-submodule-commit
make  check-recursive
make[1]: Entering directory '/sources/patch-2.7.6'
Making check in lib

===== TL;DR =====

============================================================================
Testsuite summary for GNU patch 2.7.6
============================================================================
# TOTAL: 44
# PASS:  41
# SKIP:  1
# XFAIL: 2
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

real	0m0.849s
user	0m1.681s
sys	0m0.441s

(lfs chroot) root:/sources/patch-2.7.6# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/patch-2.7.6# time make install
make  install-recursive
make[1]: Entering directory '/sources/patch-2.7.6'
Making install in lib

===== TL;DR =====

real	0m0.254s
user	0m0.158s
sys	0m0.019s

(lfs chroot) root:/sources/patch-2.7.6# 

```

<br>
### INPUT
```
cd ../
rm -rf patch-2.7.6/

```

### OUTPUT
```
(lfs chroot) root:/sources/patch-2.7.6# cd ../

(lfs chroot) root:/sources# rm -rf patch-2.7.6/

(lfs chroot) root:/sources# 

```

<br>
# Man-DB-2.9.3

### INPUT
```
tar xf man-db-2.9.3.tar.xz
cd man-db-2.9.3/
./configure --prefix=/usr                        \
            --docdir=/usr/share/doc/man-db-2.9.3 \
            --sysconfdir=/etc                    \
            --disable-setuid                     \
            --enable-cache-owner=bin             \
            --with-browser=/usr/bin/lynx         \
            --with-vgrind=/usr/bin/vgrind        \
            --with-grap=/usr/bin/grap            \
            --with-systemdtmpfilesdir=           \
            --with-systemdsystemunitdir=

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf man-db-2.9.3.tar.xz

(lfs chroot) root:/sources# cd man-db-2.9.3/

(lfs chroot) root:/sources/man-db-2.9.3# ./configure --prefix=/usr                        \
>             --docdir=/usr/share/doc/man-db-2.9.3 \
>             --sysconfdir=/etc                    \
>             --disable-setuid                     \

===== TL;DR =====

config.status: creating po/Makefile
config.status: creating gl/po/POTFILES
config.status: creating gl/po/Makefile

(lfs chroot) root:/sources/man-db-2.9.3# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/man-db-2.9.3# time make
make  all-recursive
make[1]: Entering directory '/sources/man-db-2.9.3'
Making all in docs

===== TL;DR =====

real	0m6.929s
user	0m20.815s
sys	0m2.442s

(lfs chroot) root:/sources/man-db-2.9.3# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/man-db-2.9.3# time make check
Making check in docs
make[1]: Entering directory '/sources/man-db-2.9.3/docs'
make[1]: Nothing to be done for 'check'.

===== TL;DR =====

============================================================================
Testsuite summary for man-db 2.9.3
============================================================================
# TOTAL: 29
# PASS:  29
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for man-db 2.9.3
============================================================================
# TOTAL: 9
# PASS:  9
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for man-db 2.9.3
============================================================================
# TOTAL: 12
# PASS:  12
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

real	0m8.930s
user	0m7.017s
sys	0m1.113s

(lfs chroot) root:/sources/man-db-2.9.3# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/man-db-2.9.3# time make install
Making install in docs
make[1]: Entering directory '/sources/man-db-2.9.3/docs'
make[2]: Entering directory '/sources/man-db-2.9.3/docs'

===== TL;DR =====

----------------------------------------------------------------------
Libraries have been installed in:
   /usr/lib/man-db

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

----------------------------------------------------------------------
Libraries have been installed in:
   /usr/lib/man-db

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

real	0m1.865s
user	0m1.474s
sys	0m0.510s

(lfs chroot) root:/sources/man-db-2.9.3# 

```

<br>
### INPUT
```
cd ../
rm -rf man-db-2.9.3/

```

### OUTPUT
```
(lfs chroot) root:/sources/man-db-2.9.3# cd ../

(lfs chroot) root:/sources# rm -rf man-db-2.9.3/

(lfs chroot) root:/sources# 

```

<br>
# Tar-1.32

### INPUT
```
tar xf tar-1.32.tar.xz
cd tar-1.32/
FORCE_UNSAFE_CONFIGURE=1  \
./configure --prefix=/usr \
            --bindir=/bin

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf tar-1.32.tar.xz

(lfs chroot) root:/sources# cd tar-1.32/

(lfs chroot) root:/sources/tar-1.32# FORCE_UNSAFE_CONFIGURE=1  \
> ./configure --prefix=/usr \
>             --bindir=/bin
checking for a BSD-compatible install... /usr/bin/install -c

===== TL;DR =====

config.status: creating po/POTFILES
config.status: creating po/Makefile
config.status: executing tests/atconfig commands

(lfs chroot) root:/sources/tar-1.32# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/tar-1.32# time make
make  all-recursive
make[1]: Entering directory '/sources/tar-1.32'
Making all in doc

===== TL;DR =====

real	0m5.472s
user	0m16.500s
sys	0m1.590s

(lfs chroot) root:/sources/tar-1.32#

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/tar-1.32# time make check
Making check in doc
make[1]: Entering directory '/sources/tar-1.32/doc'
make[1]: Nothing to be done for 'check'.

===== TL;DR =====

## ------------- ##
## Test results. ##
## ------------- ##

ERROR: 215 tests were run,
1 failed unexpectedly.
19 tests were skipped.
## -------------------------- ##
## testsuite.log was created. ##
## -------------------------- ##

===== TL;DR =====

real	7m11.959s
user	0m46.232s
sys	1m5.401s

(lfs chroot) root:/sources/tar-1.32# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/tar-1.32# time make install
Making install in doc
make[1]: Entering directory '/sources/tar-1.32/doc'
make[2]: Entering directory '/sources/tar-1.32/doc'

===== TL;DR =====

real	0m1.233s
user	0m0.783s
sys	0m0.280s
(lfs chroot) root:/sources/tar-1.32# 

```

<br>
### INPUT
```
make -C doc install-html docdir=/usr/share/doc/tar-1.32
cd ../
rm -rf tar-1.32/

```

### OUTPUT
```
(lfs chroot) root:/sources/tar-1.32# make -C doc install-html docdir=/usr/share/doc/tar-1.32
make: Entering directory '/sources/tar-1.32/doc'
  MAKEINFO tar.html
 /bin/mkdir -p '/usr/share/doc/tar-1.32'
 /bin/mkdir -p '/usr/share/doc/tar-1.32/tar.html'
 /usr/bin/install -c -m 644 'tar.html'/* '/usr/share/doc/tar-1.32/tar.html'
make: Leaving directory '/sources/tar-1.32/doc'

(lfs chroot) root:/sources/tar-1.32# cd ../

(lfs chroot) root:/sources# rm -rf tar-1.32/

(lfs chroot) root:/sources# 

```

<br>
# Texinfo-6.7

### INPUT
```
tar xf texinfo-6.7.tar.xz
cd texinfo-6.7/
./configure --prefix=/usr --disable-static

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf texinfo-6.7.tar.xz

(lfs chroot) root:/sources# cd texinfo-6.7/

(lfs chroot) root:/sources/texinfo-6.7# ./configure --prefix=/usr --disable-static
configure: WARNING: unrecognized options: --disable-static
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes

===== TL;DR =====

config.status: creating po_document/POTFILES
config.status: creating po_document/Makefile
configure: WARNING: unrecognized options: --disable-static

(lfs chroot) root:/sources/texinfo-6.7# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/texinfo-6.7# time make
make  all-recursive
make[1]: Entering directory '/sources/texinfo-6.7'
Making all in gnulib/lib

===== TL;DR =====

real	0m8.894s
user	0m19.673s
sys	0m1.940s

(lfs chroot) root:/sources/texinfo-6.7# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/texinfo-6.7# time make check
Making check in gnulib/lib
make[1]: Entering directory '/sources/texinfo-6.7/gnulib/lib'
make  check-recursive

===== TL;DR =====

============================================================================
Testsuite summary for GNU Texinfo 6.7
============================================================================
# TOTAL: 85
# PASS:  85
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU Texinfo 6.7
============================================================================
# TOTAL: 57
# PASS:  57
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU Texinfo 6.7
============================================================================
# TOTAL: 0
# PASS:  0
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU Texinfo 6.7
============================================================================
# TOTAL: 96
# PASS:  83
# SKIP:  13
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU Texinfo 6.7
============================================================================
# TOTAL: 2
# PASS:  0
# SKIP:  2
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU Texinfo 6.7
============================================================================
# TOTAL: 1
# PASS:  1
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU Texinfo 6.7
============================================================================
# TOTAL: 1
# PASS:  1
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

============================================================================
Testsuite summary for GNU Texinfo 6.7
============================================================================
# TOTAL: 1
# PASS:  1
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

real	0m13.435s
user	0m47.070s
sys	0m5.431s

(lfs chroot) root:/sources/texinfo-6.7# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/texinfo-6.7# time make install
Making install in gnulib/lib
make[1]: Entering directory '/sources/texinfo-6.7/gnulib/lib'
make  install-recursive

===== TL;DR =====

----------------------------------------------------------------------
Libraries have been installed in:
   /usr/lib/texinfo

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

real	0m0.796s
user	0m0.691s
sys	0m0.153s

(lfs chroot) root:/sources/texinfo-6.7# 

```

<br>
### INPUT
```
make TEXMF=/usr/share/texmf install-tex
cd ../
rm -rf texinfo-6.7/

```

### OUTPUT
```
(lfs chroot) root:/sources/texinfo-6.7# make TEXMF=/usr/share/texmf install-tex
cd doc && make TEXMF=/usr/share/texmf install-tex
make[1]: Entering directory '/sources/texinfo-6.7/doc'
test -n "/usr/share/texmf" || (echo "TEXMF must be set." >&2; exit 1)
/bin/sh /sources/texinfo-6.7/build-aux/install-sh -d /usr/share/texmf/tex/texinfo /usr/share/texmf/tex/generic/epsf
/usr/bin/install -c -m 644 ./texinfo.tex /usr/share/texmf/tex/texinfo/texinfo.tex
/usr/bin/install -c -m 644 ./texinfo-ja.tex /usr/share/texmf/tex/texinfo/texinfo-ja.tex
/usr/bin/install -c -m 644 ./epsf.tex /usr/share/texmf/tex/generic/epsf/epsf.tex
for f in txi-ca.tex txi-cs.tex txi-de.tex txi-en.tex txi-es.tex txi-fr.tex txi-hu.tex txi-is.tex txi-it.tex txi-ja.tex txi-nb.tex txi-nl.tex txi-nn.tex txi-pl.tex txi-pt.tex txi-ru.tex txi-sr.tex txi-tr.tex txi-uk.tex; do \
  /usr/bin/install -c -m 644 ./$f /usr/share/texmf/tex/texinfo/$f; done
make[1]: Leaving directory '/sources/texinfo-6.7/doc'

(lfs chroot) root:/sources/texinfo-6.7# cd ../

(lfs chroot) root:/sources# rm -rf texinfo-6.7/

```

<br>
# Vim-8.2.1361

### INPUT
```
tar xf vim-8.2.1361.tar.gz
cd vim-8.2.1361/
echo '#define SYS_VIMRC_FILE "/etc/vimrc"' >> src/feature.h
./configure --prefix=/usr

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf vim-8.2.1361.tar.gz

(lfs chroot) root:/sources# cd vim-8.2.1361/

(lfs chroot) root:/sources/vim-8.2.1361# echo '#define SYS_VIMRC_FILE "/etc/vimrc"' >> src/feature.h

(lfs chroot) root:/sources/vim-8.2.1361# ./configure --prefix=/usr
configure: creating cache auto/config.cache
checking whether make sets $(MAKE)... yes
checking for gcc... gcc

===== TL;DR =====

configure: creating auto/config.status
config.status: creating auto/config.mk
config.status: creating auto/config.h

(lfs chroot) root:/sources/vim-8.2.1361# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/vim-8.2.1361# time make
Starting make in the src directory.
If there are problems, cd to the src directory and run make there
cd src && make first

===== TL;DR =====

real	0m14.421s
user	1m17.981s
sys	0m3.518s

(lfs chroot) root:/sources/vim-8.2.1361# 

```

<br>
### INPUT
```
chown -Rv tester .

```

### OUTPUT
```
(lfs chroot) root:/sources/vim-8.2.1361# chown -Rv tester .
changed ownership of './CONTRIBUTING.md' from root to tester
changed ownership of './.hgignore' from root to tester
changed ownership of './.travis.yml' from root to tester

===== TL;DR =====

changed ownership of './src/iid_ole.c' from root to tester
changed ownership of './src' from root to tester
changed ownership of '.' from root to tester

(lfs chroot) root:/sources/vim-8.2.1361#

```

<br>
### INPUT
```
su tester -c "LANG=en_US.UTF-8 make -j1 test" &> vim-test.log

```

### OUTPUT
```
(lfs chroot) root:/sources/vim-8.2.1361# su tester -c "LANG=en_US.UTF-8 make -j1 test" &> vim-test.log

(lfs chroot) root:/sources/vim-8.2.1361# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/vim-8.2.1361# time make install
Starting make in the src directory.
If there are problems, cd to the src directory and run make there
cd src && make install

===== TL;DR =====

real	0m0.576s
user	0m0.756s
sys	0m0.251s

(lfs chroot) root:/sources/vim-8.2.1361#

```

<br>
### INPUT
```
ln -sv vim /usr/bin/vi
for L in  /usr/share/man/{,*/}man1/vim.1; do
    ln -sv vim.1 $(dirname $L)/vi.1
done
ln -sv ../vim/vim82/doc /usr/share/doc/vim-8.2.1361

cd ..
rm -rf vim-8.2.1361/

```

### OUTPUT
```
(lfs chroot) root:/sources/vim-8.2.1361# ln -sv vim /usr/bin/vi
'/usr/bin/vi' -> 'vim'

(lfs chroot) root:/sources/vim-8.2.1361# for L in  /usr/share/man/{,*/}man1/vim.1; do
>     ln -sv vim.1 $(dirname $L)/vi.1
> done
'/usr/share/man/man1/vi.1' -> 'vim.1'
'/usr/share/man/da.ISO8859-1/man1/vi.1' -> 'vim.1'
'/usr/share/man/da.UTF-8/man1/vi.1' -> 'vim.1'
'/usr/share/man/da/man1/vi.1' -> 'vim.1'
'/usr/share/man/de.ISO8859-1/man1/vi.1' -> 'vim.1'
'/usr/share/man/de.UTF-8/man1/vi.1' -> 'vim.1'
'/usr/share/man/de/man1/vi.1' -> 'vim.1'
'/usr/share/man/fr.ISO8859-1/man1/vi.1' -> 'vim.1'
'/usr/share/man/fr.UTF-8/man1/vi.1' -> 'vim.1'
'/usr/share/man/fr/man1/vi.1' -> 'vim.1'
'/usr/share/man/it.ISO8859-1/man1/vi.1' -> 'vim.1'
'/usr/share/man/it.UTF-8/man1/vi.1' -> 'vim.1'
'/usr/share/man/it/man1/vi.1' -> 'vim.1'
'/usr/share/man/ja/man1/vi.1' -> 'vim.1'
'/usr/share/man/pl.ISO8859-2/man1/vi.1' -> 'vim.1'
'/usr/share/man/pl.UTF-8/man1/vi.1' -> 'vim.1'
'/usr/share/man/pl/man1/vi.1' -> 'vim.1'
'/usr/share/man/ru.KOI8-R/man1/vi.1' -> 'vim.1'
'/usr/share/man/ru.UTF-8/man1/vi.1' -> 'vim.1'
'/usr/share/man/tr.ISO8859-9/man1/vi.1' -> 'vim.1'
'/usr/share/man/tr.UTF-8/man1/vi.1' -> 'vim.1'
'/usr/share/man/tr/man1/vi.1' -> 'vim.1'

(lfs chroot) root:/sources/vim-8.2.1361# ln -sv ../vim/vim82/doc /usr/share/doc/vim-8.2.1361
'/usr/share/doc/vim-8.2.1361' -> '../vim/vim82/doc'

(lfs chroot) root:/sources/vim-8.2.1361# cd ..

(lfs chroot) root:/sources# rm -rf vim-8.2.1361/

(lfs chroot) root:/sources# 

```

<br>
# Eudev-3.2.9

### INPUT
```
tar xf eudev-3.2.9.tar.gz
cd eudev-3.2.9/
./configure --prefix=/usr           \
            --bindir=/sbin          \
            --sbindir=/sbin         \
            --libdir=/usr/lib       \
            --sysconfdir=/etc       \
            --libexecdir=/lib       \
            --with-rootprefix=      \
            --with-rootlibdir=/lib  \
            --enable-manpages       \
            --disable-static

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf eudev-3.2.9.tar.gz

(lfs chroot) root:/sources# cd eudev-3.2.9/

(lfs chroot) root:/sources/eudev-3.2.9# ./configure --prefix=/usr           \
>             --bindir=/sbin          \
>             --sbindir=/sbin         \
>             --libdir=/usr/lib       \

===== TL;DR =====

        prefix:                  /usr
        exec_prefix:             ${prefix}
        sysconfdir:              /etc
        datadir:                 ${datarootdir}
        includedir:              ${prefix}/include
        bindir:                  /sbin
        libdir:                  /usr/lib

        rootprefix:              
        rootlibdir:              /lib
        rootlibexecdir:          /lib/udev
        datarootdir:             ${prefix}/share
        rootrundir:              /run

        udevconfdir:             /etc/udev
        udevconffile:            /etc/udev/udev.conf
        udevhwdbdir:             /etc/udev/hwdb.d
        udevhwdbbin:             /etc/udev/hwdb.bin
        udevlibexecdir:          /lib/udev
        udevkeymapdir:           /lib/udev/keymaps
        udevkeymapforceredir:    /lib/udev/keymaps/force-release
        udevrulesdir:            /lib/udev/rules.d

        pkgconfiglibdir:         /usr/lib/pkgconfig
        sharepkgconfigdir        ${datarootdir}/pkgconfig

        girdir                   ${datarootdir}/gir-1.0
        typelibsdir              /usr/lib/girepository-1.0

(lfs chroot) root:/sources/eudev-3.2.9# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```

(lfs chroot) root:/sources/eudev-3.2.9# time make
make  all-recursive
make[1]: Entering directory '/sources/eudev-3.2.9'
Making all in src

===== TL;DR =====

real	0m5.535s
user	0m13.960s
sys	0m1.654s

(lfs chroot) root:/sources/eudev-3.2.9# 

```

<br>
### INPUT
```
mkdir -pv /lib/udev/rules.d
mkdir -pv /etc/udev/rules.d
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/eudev-3.2.9# mkdir -pv /lib/udev/rules.d
mkdir: created directory '/lib/udev'
mkdir: created directory '/lib/udev/rules.d'

(lfs chroot) root:/sources/eudev-3.2.9# mkdir -pv /etc/udev/rules.d
mkdir: created directory '/etc/udev'
mkdir: created directory '/etc/udev/rules.d'

(lfs chroot) root:/sources/eudev-3.2.9# time make check
Making check in src
make[1]: Entering directory '/sources/eudev-3.2.9/src'
Making check in shared

===== TL;DR =====

============================================================================
Testsuite summary for eudev 3.2.9
============================================================================
# TOTAL: 2
# PASS:  2
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

real	0m8.115s
user	0m1.374s
sys	0m1.402s

(lfs chroot) root:/sources/eudev-3.2.9# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/eudev-3.2.9# time make install
Making install in src
make[1]: Entering directory '/sources/eudev-3.2.9/src'
Making install in shared

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

real	0m1.057s
user	0m0.682s
sys	0m0.155s

(lfs chroot) root:/sources/eudev-3.2.9#

```

<br>
### INPUT
```
tar -xvf ../udev-lfs-20171102.tar.xz
make -f udev-lfs-20171102/Makefile.lfs install

```

### OUTPUT
```
(lfs chroot) root:/sources/eudev-3.2.9# tar -xvf ../udev-lfs-20171102.tar.xz
udev-lfs-20171102/
udev-lfs-20171102/init-net-rules.sh
udev-lfs-20171102/83-cdrom-symlinks.rules

===== TL;DR =====

udev-lfs-20171102/rule_generator.functions
udev-lfs-20171102/55-lfs.rules
udev-lfs-20171102/Makefile.lfs

(lfs chroot) root:/sources/eudev-3.2.9# make -f udev-lfs-20171102/Makefile.lfs install
mkdir: created directory '/usr/share/doc/udev-20171102'
mkdir: created directory '/usr/share/doc/udev-20171102/lfs'
'udev-lfs-20171102/55-lfs.rules' -> '/etc/udev/rules.d/55-lfs.rules'
'udev-lfs-20171102/81-cdrom.rules' -> '/etc/udev/rules.d/81-cdrom.rules'
'udev-lfs-20171102/83-cdrom-symlinks.rules' -> '/etc/udev/rules.d/83-cdrom-symlinks.rules'
'udev-lfs-20171102/write_cd_rules' -> '/lib/udev/write_cd_rules'
'udev-lfs-20171102/write_net_rules' -> '/lib/udev/write_net_rules'
'udev-lfs-20171102/init-net-rules.sh' -> '/lib/udev/init-net-rules.sh'
'udev-lfs-20171102/rule_generator.functions' -> '/lib/udev/rule_generator.functions'
'udev-lfs-20171102/README' -> '/usr/share/doc/udev-20171102/lfs/README'
'udev-lfs-20171102/55-lfs.txt' -> '/usr/share/doc/udev-20171102/lfs/55-lfs.txt'

(lfs chroot) root:/sources/eudev-3.2.9# 

```

<br>
### INPUT
```
udevadm hwdb --update
cd ../
rm -rf eudev-3.2.9/

```

### OUTPUT
```
(lfs chroot) root:/sources/eudev-3.2.9# udevadm hwdb --update

(lfs chroot) root:/sources/eudev-3.2.9# cd ../

(lfs chroot) root:/sources# rm -rf eudev-3.2.9/

(lfs chroot) root:/sources# 

```

<br>
# Procps-ng-3.3.16

### INPUT
```
tar xf procps-ng-3.3.16.tar.xz
cd procps-ng-3.3.16/
./configure --prefix=/usr                            \
            --exec-prefix=                           \
            --libdir=/usr/lib                        \
            --docdir=/usr/share/doc/procps-ng-3.3.16 \
            --disable-static                         \
            --disable-kill

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf procps-ng-3.3.16.tar.xz

(lfs chroot) root:/sources# cd procps-ng-3.3.16/

(lfs chroot) root:/sources/procps-ng-3.3.16# ./configure --prefix=/usr                            \
>             --exec-prefix=                           \
>             --libdir=/usr/lib                        \
>             --docdir=/usr/share/doc/procps-ng-3.3.16 \

===== TL;DR =====

config.status: executing default-1 commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

(lfs chroot) root:/sources/procps-ng-3.3.16# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/procps-ng-3.3.16# time make
make  all-recursive
make[1]: Entering directory '/sources/procps-ng-3.3.16'
Making all in include

===== TL;DR =====

real	0m3.125s
user	0m9.003s
sys	0m0.981s

(lfs chroot) root:/sources/procps-ng-3.3.16# 

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/procps-ng-3.3.16# time make check
make  check-recursive
make[1]: Entering directory '/sources/procps-ng-3.3.16'
Making check in include

===== TL;DR =====

============================================================================
Testsuite summary for procps-ng 3.3.16
============================================================================
# TOTAL: 1
# PASS:  1
# SKIP:  0
# XFAIL: 0
# FAIL:  0
# XPASS: 0
# ERROR: 0
============================================================================

===== TL;DR =====

real	0m4.857s
user	0m0.967s
sys	0m0.307s

(lfs chroot) root:/sources/procps-ng-3.3.16# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/procps-ng-3.3.16# time make install
make  install-recursive
make[1]: Entering directory '/sources/procps-ng-3.3.16'
Making install in include

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

real	0m0.580s
user	0m0.583s
sys	0m0.103s

(lfs chroot) root:/sources/procps-ng-3.3.16# 

```

<br>
### INPUT
```
mv -v /usr/lib/libprocps.so.* /lib
ln -sfv ../../lib/$(readlink /usr/lib/libprocps.so) /usr/lib/libprocps.so
cd ../
rm -rf procps-ng-3.3.16/

```

### OUTPUT
```
(lfs chroot) root:/sources/procps-ng-3.3.16# mv -v /usr/lib/libprocps.so.* /lib
renamed '/usr/lib/libprocps.so.8' -> '/lib/libprocps.so.8'
renamed '/usr/lib/libprocps.so.8.0.2' -> '/lib/libprocps.so.8.0.2'

(lfs chroot) root:/sources/procps-ng-3.3.16# ln -sfv ../../lib/$(readlink /usr/lib/libprocps.so) /usr/lib/libprocps.so
'/usr/lib/libprocps.so' -> '../../lib/libprocps.so.8.0.2'

(lfs chroot) root:/sources/procps-ng-3.3.16# cd ../

(lfs chroot) root:/sources# rm -rf procps-ng-3.3.16/

(lfs chroot) root:/sources# 

```

<br>
# Util-linux-2.36

### INPUT
```
tar xf util-linux-2.36.tar.xz
cd util-linux-2.36/
mkdir -pv /var/lib/hwclock
./configure ADJTIME_PATH=/var/lib/hwclock/adjtime   \
            --docdir=/usr/share/doc/util-linux-2.36 \
            --disable-chfn-chsh  \
            --disable-login      \
            --disable-nologin    \
            --disable-su         \
            --disable-setpriv    \
            --disable-runuser    \
            --disable-pylibmount \
            --disable-static     \
            --without-python     \
            --without-systemd    \
            --without-systemdsystemunitdir

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf util-linux-2.36.tar.xz

(lfs chroot) root:/sources# cd util-linux-2.36/

(lfs chroot) root:/sources/util-linux-2.36# mkdir -pv /var/lib/hwclock

(lfs chroot) root:/sources/util-linux-2.36# ./configure ADJTIME_PATH=/var/lib/hwclock/adjtime   \
>             --docdir=/usr/share/doc/util-linux-2.36 \
>             --disable-chfn-chsh  \
>             --disable-login      \

===== TL;DR =====

	util-linux  2.36

	prefix:            /usr
	exec prefix:       ${prefix}

	runstatedir:       ${localstatedir}/run
	bindir:            /bin
	sbindir:           /sbin
	libdir:            /lib
	includedir:        ${prefix}/include
	usrbin_execdir:    ${exec_prefix}/bin
	usrsbin_execdir:   ${exec_prefix}/sbin
	usrlib_execdir:    ${exec_prefix}/lib
        vendordir:         

	compiler:          gcc
	cflags:            -g -O2
	suid cflags:       
	ldflags:           
	suid ldflags:      
	ASAN enabled:      no

	Python:            
	Python version:    
	Python libs:       

	Bash completions:  ${datarootdir}/bash-completion/completions
	Systemd support:   no
	Systemd unitdir:   no
	libeconf support:  no
	Btrfs support:     yes
	Wide-char support: yes

	warnings:

 -fno-common -Wall -Wextra -Wmissing-declarations -Wmissing-parameter-type -Wmissing-prototypes -Wno-missing-field-initializers -Wredundant-decls -Wsign-compare -Wtype-limits -Wuninitialized -Wunused-but-set-parameter -Wunused-but-set-variable -Wunused-parameter -Wunused-result -Wunused-variable -Wnested-externs -Wpointer-arith -Wstrict-prototypes -Wimplicit-function-declaration -Wdiscarded-qualifiers -Waddress-of-packed-member -Werror=sequence-point

	Type 'make' or 'make <utilname>' to compile.

(lfs chroot) root:/sources/util-linux-2.36# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/util-linux-2.36# time make
make  all-recursive
make[1]: Entering directory '/sources/util-linux-2.36'
Making all in po

===== TL;DR =====

real	0m19.931s
user	1m25.559s
sys	0m11.531s

(lfs chroot) root:/sources/util-linux-2.36#

```

<br>
### INPUT
```
chown -Rv tester .
su tester -c "make -k check"

```

### OUTPUT
```
(lfs chroot) root:/sources/util-linux-2.36# chown -Rv tester .
changed ownership of './ipcmk' from root to tester
changed ownership of './mkfs.minix' from root to tester
changed ownership of './README.licensing' from 1000 to tester

===== TL;DR =====

changed ownership of './getopt' from root to tester
changed ownership of './lsns' from root to tester
changed ownership of '.' from 1000 to tester

(lfs chroot) root:/sources/util-linux-2.36# su tester -c "make -k check"
make  check-recursive
make[1]: Entering directory '/sources/util-linux-2.36'
Making check in po

===== TL;DR =====

---------------------------------------------------------------------
  All 207 tests PASSED
---------------------------------------------------------------------
make[3]: Leaving directory '/sources/util-linux-2.36'
make[2]: Leaving directory '/sources/util-linux-2.36'
make[1]: Leaving directory '/sources/util-linux-2.36'

(lfs chroot) root:/sources/util-linux-2.36# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/util-linux-2.36# time make install
make  install-recursive
make[1]: Entering directory '/sources/util-linux-2.36'
Making install in po

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

real	0m1.520s
user	0m2.508s
sys	0m0.560s

(lfs chroot) root:/sources/util-linux-2.36#

```

<br>
### INPUT
```
cd ../
rm -rf util-linux-2.36/

```

### OUTPUT
```
(lfs chroot) root:/sources/util-linux-2.36# cd ../

(lfs chroot) root:/sources# rm -rf util-linux-2.36/

(lfs chroot) root:/sources# 

```

<br>
# E2fsprogs-1.45.6

### INPUT
```
tar xf e2fsprogs-1.45.6.tar.gz
cd e2fsprogs-1.45.6/
mkdir -v build
cd       build
../configure --prefix=/usr           \
             --bindir=/bin           \
             --with-root-prefix=""   \
             --enable-elf-shlibs     \
             --disable-libblkid      \
             --disable-libuuid       \
             --disable-uuidd         \
             --disable-fsck

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf e2fsprogs-1.45.6.tar.gz

(lfs chroot) root:/sources# cd e2fsprogs-1.45.6/

(lfs chroot) root:/sources/e2fsprogs-1.45.6# mkdir -v build
mkdir: created directory 'build'

(lfs chroot) root:/sources/e2fsprogs-1.45.6# cd       build

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# ../configure --prefix=/usr           \
>              --bindir=/bin           \
>              --with-root-prefix=""   \
>              --enable-elf-shlibs     \

===== TL;DR =====

config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# time make
cd ./util ; make subst
make[1]: Entering directory '/sources/e2fsprogs-1.45.6/build/util'
	CREATE dirpaths.h

===== TL;DR =====

real	0m11.481s
user	0m41.484s
sys	0m4.455s

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build#

```

<br>
### INPUT
```
time make check

```

### OUTPUT
```
(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# time make check
make[1]: Entering directory '/sources/e2fsprogs-1.45.6/build'
make[1]: 'util/subst.conf' is up to date.
make[1]: Leaving directory '/sources/e2fsprogs-1.45.6/build'

===== TL;DR =====

357 tests succeeded	0 tests failed

real	13m46.076s
user	1m4.016s
sys	0m23.288s

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# time make install
making install-shlibs in lib/et
make[1]: Entering directory '/sources/e2fsprogs-1.45.6/build/doc'
make[1]: Entering directory '/sources/e2fsprogs-1.45.6/build'

===== TL;DR =====

real	0m4.530s
user	0m1.173s
sys	0m0.498s

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build#

```

<br>
### INPUT
```
chmod -v u+w /usr/lib/{libcom_err,libe2p,libext2fs,libss}.a
gunzip -v /usr/share/info/libext2fs.info.gz
install-info --dir-file=/usr/share/info/dir /usr/share/info/libext2fs.info

```

### OUTPUT
```
(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# chmod -v u+w /usr/lib/{libcom_err,libe2p,libext2fs,libss}.a
mode of '/usr/lib/libcom_err.a' changed from 0444 (r--r--r--) to 0644 (rw-r--r--)
mode of '/usr/lib/libe2p.a' changed from 0444 (r--r--r--) to 0644 (rw-r--r--)
mode of '/usr/lib/libext2fs.a' changed from 0444 (r--r--r--) to 0644 (rw-r--r--)
mode of '/usr/lib/libss.a' changed from 0444 (r--r--r--) to 0644 (rw-r--r--)

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# gunzip -v /usr/share/info/libext2fs.info.gz
/usr/share/info/libext2fs.info.gz:	 79.7% -- replaced with /usr/share/info/libext2fs.info

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# install-info --dir-file=/usr/share/info/dir /usr/share/info/libext2fs.info

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# 

```

<br>
### INPUT
```
makeinfo -o      doc/com_err.info ../lib/et/com_err.texinfo
install -v -m644 doc/com_err.info /usr/share/info
install-info --dir-file=/usr/share/info/dir /usr/share/info/com_err.info

```

### OUTPUT
```
(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# makeinfo -o      doc/com_err.info ../lib/et/com_err.texinfo

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# install -v -m644 doc/com_err.info /usr/share/info
'doc/com_err.info' -> '/usr/share/info/com_err.info'

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# install-info --dir-file=/usr/share/info/dir /usr/share/info/com_err.info

(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# 

```

<br>
### INPUT
```
cd ../../
rm -rf e2fsprogs-1.45.6/

```

### OUTPUT
```
(lfs chroot) root:/sources/e2fsprogs-1.45.6/build# cd ../../

(lfs chroot) root:/sources# rm -rf e2fsprogs-1.45.6/

(lfs chroot) root:/sources# 

```

<br>
# Sysklogd-1.5.1

### INPUT
```
tar xf sysklogd-1.5.1.tar.gz
cd sysklogd-1.5.1/
sed -i '/Error loading kernel symbols/{n;n;d}' ksym_mod.c
sed -i 's/union wait/int/' syslogd.c

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf sysklogd-1.5.1.tar.gz

(lfs chroot) root:/sources# cd sysklogd-1.5.1/

(lfs chroot) root:/sources/sysklogd-1.5.1# sed -i '/Error loading kernel symbols/{n;n;d}' ksym_mod.c

(lfs chroot) root:/sources/sysklogd-1.5.1# sed -i 's/union wait/int/' syslogd.c

(lfs chroot) root:/sources/sysklogd-1.5.1# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/sysklogd-1.5.1# time make
gcc  -O3 -DSYSV -fomit-frame-pointer -Wall -fno-strength-reduce -DSYSLOG_INET -DSYSLOG_UNIXAF -DNO_SCCS -DFSSTND -DSYSLOGD_PIDNAME=\"syslogd.pid\"  -c syslogd.c
gcc    -c -o pidfile.o pidfile.c

===== TL;DR =====

real	0m0.478s
user	0m0.719s
sys	0m0.063s

(lfs chroot) root:/sources/sysklogd-1.5.1#

```

<br>
### INPUT
```
time make BINDIR=/sbin install

```

### OUTPUT
```
(lfs chroot) root:/sources/sysklogd-1.5.1# time make BINDIR=/sbin install
/usr/bin/install -o root -g root -m 644 sysklogd.8 /usr/share/man/man8/sysklogd.8
/usr/bin/install -m 500 -s syslogd /sbin/syslogd
/usr/bin/install -o root -g root -m 644 syslogd.8 /usr/share/man/man8/syslogd.8
/usr/bin/install -o root -g root -m 644 syslog.conf.5 /usr/share/man/man5/syslog.conf.5
/usr/bin/install -o root -g root -m 644 klogd.8 /usr/share/man/man8/klogd.8
/usr/bin/install -m 500 -s klogd /sbin/klogd

real	0m0.007s
user	0m0.010s
sys	0m0.001s
(lfs chroot) root:/sources/sysklogd-1.5.1# 

```

<br>
### INPUT
```
cat > /etc/syslog.conf << "EOF"
# Begin /etc/syslog.conf

auth,authpriv.* -/var/log/auth.log
*.*;auth,authpriv.none -/var/log/sys.log
daemon.* -/var/log/daemon.log
kern.* -/var/log/kern.log
mail.* -/var/log/mail.log
user.* -/var/log/user.log
*.emerg *

# End /etc/syslog.conf
EOF

```

### OUTPUT
```
(lfs chroot) root:/sources/sysklogd-1.5.1# cat > /etc/syslog.conf << "EOF"
> # Begin /etc/syslog.conf
> 
> auth,authpriv.* -/var/log/auth.log
> *.*;auth,authpriv.none -/var/log/sys.log
> daemon.* -/var/log/daemon.log
> kern.* -/var/log/kern.log
> mail.* -/var/log/mail.log
> user.* -/var/log/user.log
> *.emerg *
> 
> # End /etc/syslog.conf
> EOF

(lfs chroot) root:/sources/sysklogd-1.5.1# 

```

<br>
### INPUT
```
cd ../
rm -rf sysklogd-1.5.1/

```

### OUTPUT
```
(lfs chroot) root:/sources/sysklogd-1.5.1# cd ../

(lfs chroot) root:/sources# rm -rf sysklogd-1.5.1/

(lfs chroot) root:/sources# 

```

<br>
# Sysvinit-2.97

### INPUT
```
tar xf sysvinit-2.97.tar.xz
cd sysvinit-2.97/
patch -Np1 -i ../sysvinit-2.97-consolidated-1.patch

```

### OUTPUT
```
(lfs chroot) root:/sources# tar xf sysvinit-2.97.tar.xz

(lfs chroot) root:/sources# cd sysvinit-2.97/

(lfs chroot) root:/sources/sysvinit-2.97# patch -Np1 -i ../sysvinit-2.97-consolidated-1.patch
patching file src/Makefile
Hunk #2 succeeded at 211 (offset 2 lines).
Hunk #3 succeeded at 236 (offset 2 lines).

(lfs chroot) root:/sources/sysvinit-2.97# 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
(lfs chroot) root:/sources/sysvinit-2.97# time make
make VERSION=2.97 -C src all
make[1]: Entering directory '/sources/sysvinit-2.97/src'
cc -O2 -ansi -fomit-frame-pointer -fstack-protector-strong -W -Wall -Wunreachable-code -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2 -D_XOPEN_SOURCE -D_GNU_SOURCE -DVERSION=\"2.97\"    -c -o init.o init.c

===== TL;DR =====

real	0m0.820s
user	0m1.750s
sys	0m0.245s

(lfs chroot) root:/sources/sysvinit-2.97# 

```

<br>
### INPUT
```
time make install

```

### OUTPUT
```
(lfs chroot) root:/sources/sysvinit-2.97# time make install
make VERSION=2.97 -C src install
make[1]: Entering directory '/sources/sysvinit-2.97/src'
install -m 755 -d /bin/ /sbin/

===== TL;DR =====

real	0m0.062s
user	0m0.051s
sys	0m0.014s

(lfs chroot) root:/sources/sysvinit-2.97# 

```

<br>
### INPUT
```
cd ../
rm -rf sysvinit-2.97/

```

### OUTPUT
```
(lfs chroot) root:/sources/sysvinit-2.97# cd ../

(lfs chroot) root:/sources# rm -rf sysvinit-2.97/

(lfs chroot) root:/sources# 

```

<br>
# Stripping Again

### INPUT
```
df -h

```

### OUTPUT
```
(lfs chroot) root:/sources# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb1        32G  4.7G   26G  16% /
udev            3.9G     0  3.9G   0% /dev
tmpfs           3.9G     0  3.9G   0% /run

(lfs chroot) root:/sources# 

```

<br>
### INPUT
```
save_lib="ld-2.32.so libc-2.32.so libpthread-2.32.so libthread_db-1.0.so"

cd /lib

for LIB in $save_lib; do
    objcopy --only-keep-debug $LIB $LIB.dbg 
    strip --strip-unneeded $LIB
    objcopy --add-gnu-debuglink=$LIB.dbg $LIB 
done    

save_usrlib="libquadmath.so.0.0.0 libstdc++.so.6.0.28
             libitm.so.1.0.0 libatomic.so.1.2.0" 

cd /usr/lib

for LIB in $save_usrlib; do
    objcopy --only-keep-debug $LIB $LIB.dbg
    strip --strip-unneeded $LIB
    objcopy --add-gnu-debuglink=$LIB.dbg $LIB
done

unset LIB save_lib save_usrlib

```

### OUTPUT
```
(lfs chroot) root:/sources# save_lib="ld-2.32.so libc-2.32.so libpthread-2.32.so libthread_db-1.0.so"

(lfs chroot) root:/sources# cd /lib

(lfs chroot) root:/lib# for LIB in $save_lib; do
>     objcopy --only-keep-debug $LIB $LIB.dbg 
>     strip --strip-unneeded $LIB
>     objcopy --add-gnu-debuglink=$LIB.dbg $LIB 
> done    

(lfs chroot) root:/lib# save_usrlib="libquadmath.so.0.0.0 libstdc++.so.6.0.28
>              libitm.so.1.0.0 libatomic.so.1.2.0" 

(lfs chroot) root:/lib# cd /usr/lib

(lfs chroot) root:/usr/lib# for LIB in $save_usrlib; do
>     objcopy --only-keep-debug $LIB $LIB.dbg
>     strip --strip-unneeded $LIB
>     objcopy --add-gnu-debuglink=$LIB.dbg $LIB
> done

(lfs chroot) root:/usr/lib# unset LIB save_lib save_usrlib

```

<br>
### INPUT
```
find /usr/lib -type f -name \*.a \
   -exec strip --strip-debug {} ';'

find /lib /usr/lib -type f -name \*.so* ! -name \*dbg \
   -exec strip --strip-unneeded {} ';'

find /{bin,sbin} /usr/{bin,sbin,libexec} -type f \
    -exec strip --strip-all {} ';'

```

### OUTPUT
```
(lfs chroot) root:/usr/lib# find /usr/lib -type f -name \*.a \
>    -exec strip --strip-debug {} ';'
strip: /usr/lib/libm.a: file format not recognized

(lfs chroot) root:/usr/lib# find /lib /usr/lib -type f -name \*.so* ! -name \*dbg \
>    -exec strip --strip-unneeded {} ';'
strip: /usr/lib/libcursesw.so: file format not recognized
strip: /usr/lib/libgcc_s.so: file format not recognized
strip: /usr/lib/libform.so: file format not recognized
strip: /usr/lib/libpanel.so: file format not recognized
strip: /usr/lib/libncurses.so: file format not recognized
strip: /usr/lib/libm.so: file format not recognized
strip: /usr/lib/libmenu.so: file format not recognized
strip: /usr/lib/libc.so: file format not recognized

(lfs chroot) root:/usr/lib# find /{bin,sbin} /usr/{bin,sbin,libexec} -type f \
>     -exec strip --strip-all {} ';'
strip: /bin/mk_cmds: file format not recognized
strip: /bin/egrep: file format not recognized
strip: /bin/compile_et: file format not recognized

===== TL;DR =====

strip: /usr/libexec/gcc/x86_64-pc-linux-gnu/10.2.0/install-tools/fixinc.sh: file format not recognized
strip: /usr/libexec/gcc/x86_64-pc-linux-gnu/10.2.0/install-tools/mkinstalldirs: file format not recognized
strip: /usr/libexec/gcc/x86_64-pc-linux-gnu/10.2.0/install-tools/mkheaders: file format not recognized

(lfs chroot) root:/usr/lib# 

```

<br>
### INPUT
```
df -h

```

### OUTPUT
```
(lfs chroot) root:/usr/lib# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb1        32G  2.6G   28G   9% /
udev            3.9G     0  3.9G   0% /dev
tmpfs           3.9G     0  3.9G   0% /run

(lfs chroot) root:/usr/lib# 

```

<br>
# Cleaning Up

### INPUT
```
rm -rf /tmp/*
logout

```

### OUTPUT
```
(lfs chroot) root:/usr/lib# rm -rf /tmp/*

(lfs chroot) root:/usr/lib# logout

root:~# 

```

<br>
### INPUT
```
chroot "$LFS" /usr/bin/env -i          \
    HOME=/root TERM="$TERM"            \
    PS1='(lfs chroot) \u:\w\$ '        \
    PATH=/bin:/usr/bin:/sbin:/usr/sbin \
    MAKEFLAGS='-j6'                    \
    /bin/bash --login

```

### OUTPUT
```
root:~# chroot "$LFS" /usr/bin/env -i    \
>     HOME=/root TERM="$TERM"            \
>     PS1='(lfs chroot) \u:\w\$ '        \
>     PATH=/bin:/usr/bin:/sbin:/usr/sbin \
>     MAKEFLAGS='-j6'                    \
>     /bin/bash --login

(lfs chroot) root:/# 

```

<br>
### INPUT
```
rm -f /usr/lib/lib{bfd,opcodes}.a
rm -f /usr/lib/libctf{,-nobfd}.a
rm -f /usr/lib/libbz2.a
rm -f /usr/lib/lib{com_err,e2p,ext2fs,ss}.a
rm -f /usr/lib/libltdl.a
rm -f /usr/lib/libfl.a
rm -f /usr/lib/libz.a

```

### OUTPUT
```
(lfs chroot) root:/# rm -f /usr/lib/lib{bfd,opcodes}.a

(lfs chroot) root:/# rm -f /usr/lib/libctf{,-nobfd}.a

(lfs chroot) root:/# rm -f /usr/lib/libbz2.a

(lfs chroot) root:/# rm -f /usr/lib/lib{com_err,e2p,ext2fs,ss}.a

(lfs chroot) root:/# rm -f /usr/lib/libltdl.a

(lfs chroot) root:/# rm -f /usr/lib/libfl.a

(lfs chroot) root:/# rm -f /usr/lib/libz.a

(lfs chroot) root:/# 

```

<br>
### INPUT
```
find /usr/lib /usr/libexec -name \*.la -delete

```

### OUTPUT
```
(lfs chroot) root:/# find /usr/lib /usr/libexec -name \*.la -delete

(lfs chroot) root:/# 

```

<br>
### INPUT
```
find /usr -depth -name $(uname -m)-lfs-linux-gnu\* | xargs rm -rf

```

### OUTPUT
```
(lfs chroot) root:/# find /usr -depth -name $(uname -m)-lfs-linux-gnu\* | xargs rm -rf

(lfs chroot) root:/# 

```

<br>
### INPUT
```
rm -rf /tools

```

### OUTPUT
```
(lfs chroot) root:/# rm -rf /tools

(lfs chroot) root:/# 

```

<br>
### INPUT
```
userdel -r tester

```

### OUTPUT
```
(lfs chroot) root:/# userdel -r tester
userdel: tester mail spool (/var/mail/tester) not found

(lfs chroot) root:/# 

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

