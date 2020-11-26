---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-07.md)
[NEXT](index.md)

# LFS: Chapter 6

<br>
<img src="pictures/LFS-A41.jpg" width="960">

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
cd $LFS/sources/

```

### OUTPUT
```
lfs:~$ echo $LFS
/mnt/lfs

lfs:~$ cd $LFS/sources/

```

<br>
# Installation of M4

### INPUT
```
tar xf m4-1.4.18.tar.xz
cd m4-1.4.18
sed -i 's/IO_ftrylockfile/IO_EOF_SEEN/' lib/*.c
echo "#define _IO_IN_BACKUP 0x100" >> lib/stdio-impl.h
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(build-aux/config.guess)

```

### OUTPUT
```
lfs:/mnt/lfs/sources$ tar xf m4-1.4.18.tar.xz

lfs:/mnt/lfs/sources$ cd m4-1.4.18

lfs:/mnt/lfs/sources/m4-1.4.18$ sed -i 's/IO_ftrylockfile/IO_EOF_SEEN/' lib/*.c

lfs:/mnt/lfs/sources/m4-1.4.18$ echo "#define _IO_IN_BACKUP 0x100" >> lib/stdio-impl.h

lfs:/mnt/lfs/sources/m4-1.4.18$ ./configure --prefix=/usr   \
>             --host=$LFS_TGT \
>             --build=$(build-aux/config.guess)

===== TL;DR =====

config.status: creating lib/config.h
config.status: executing depfiles commands
config.status: executing stamp-h commands

lfs:/mnt/lfs/sources/m4-1.4.18$

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
lfs:/mnt/lfs/sources/m4-1.4.18$ time make
make  all-recursive
make[1]: Entering directory '/mnt/lfs/sources/m4-1.4.18'
Making all in .

===== TL;DR =====

real	0m3.821s
user	0m8.962s
sys	0m1.446s

```

<br>
### INPUT
```
time make DESTDIR=$LFS install
cd ..
rm -rf m4-1.4.18

```

### OUTPUT
```
lfs:/mnt/lfs/sources/m4-1.4.18$ time make DESTDIR=$LFS install
make  install-recursive
make[1]: Entering directory '/mnt/lfs/sources/m4-1.4.18'
Making install in .

===== TL;DR =====

make[3]: Leaving directory '/mnt/lfs/sources/m4-1.4.18/tests'
make[2]: Leaving directory '/mnt/lfs/sources/m4-1.4.18/tests'
make[1]: Leaving directory '/mnt/lfs/sources/m4-1.4.18'

lfs:/mnt/lfs/sources/m4-1.4.18$ cd ..

lfs:/mnt/lfs/sources$ rm -rf m4-1.4.18

lfs:/mnt/lfs/sources$ 

```

<br>
# Ncurses-6.2

### INPUT
```
tar xf ncurses-6.2.tar.gz
cd ncurses-6.2
sed -i s/mawk// configure
mkdir build
pushd build
  ../configure
  make -C include
  make -C progs tic
popd
./configure --prefix=/usr                \
            --host=$LFS_TGT              \
            --build=$(./config.guess)    \
            --mandir=/usr/share/man      \
            --with-manpage-format=normal \
            --with-shared                \
            --without-debug              \
            --without-ada                \
            --without-normal             \
            --enable-widec

```

### OUTPUT
```
lfs:/mnt/lfs/sources$ tar xf ncurses-6.2.tar.gz

lfs:/mnt/lfs/sources$ cd ncurses-6.2

lfs:/mnt/lfs/sources/ncurses-6.2$ sed -i s/mawk// configure

lfs:/mnt/lfs/sources/ncurses-6.2$ mkdir build

lfs:/mnt/lfs/sources/ncurses-6.2$ pushd build
/mnt/lfs/sources/ncurses-6.2/build /mnt/lfs/sources/ncurses-6.2

lfs:/mnt/lfs/sources/ncurses-6.2/build$   ../configure
checking for egrep... grep -E
Configuring NCURSES 6.2 ABI 6 (Thu Nov 26 16:59:40 WIB 2020)
checking for package version... 6.2

===== TL;DR =====

    include directory: /usr/include
        man directory: /usr/share/man
   terminfo directory: /usr/share/terminfo

lfs:/mnt/lfs/sources/ncurses-6.2/build$   make -C include
make: Entering directory '/mnt/lfs/sources/ncurses-6.2/build/include'
cat curses.head >curses.h
/bin/sh ../../include/MKhashsize.sh ../../include/Caps ../../include/Caps-ncurses >hashsize.h

===== TL;DR =====

/bin/sh -c 'if test "chtype" = "cchar_t" ; then cat ../../include/curses.wide >>curses.h ; fi'
cat ../../include/curses.tail >>curses.h
make: Leaving directory '/mnt/lfs/sources/ncurses-6.2/build/include'

lfs:/mnt/lfs/sources/ncurses-6.2/build$   make -C progs tic
make: Entering directory '/mnt/lfs/sources/ncurses-6.2/build/progs'
echo "#ifndef __TRANSFORM_H"					>transform.h
/bin/sh ../../progs/MKtermsort.sh gawk ../../progs/../include/Caps >termsort.c

===== TL;DR =====

make[1]: Leaving directory '/mnt/lfs/sources/ncurses-6.2/build/ncurses'
gcc ../objects/tic.o ../objects/dump_entry.o ../objects/tparm_type.o
make: Leaving directory '/mnt/lfs/sources/ncurses-6.2/build/progs'


lfs:/mnt/lfs/sources/ncurses-6.2/build$ popd
/mnt/lfs/sources/ncurses-6.2

lfs:/mnt/lfs/sources/ncurses-6.2$ ./configure --prefix=/usr                \
>             --host=$LFS_TGT              \
>             --build=$(./config.guess)    \
>             --mandir=/usr/share/man      \

===== TL;DR =====

        lib directory: /usr/lib
    include directory: /usr/include
        man directory: /usr/share/man
   terminfo directory: /usr/share/terminfo

lfs:/mnt/lfs/sources/ncurses-6.2$

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
lfs:/mnt/lfs/sources/ncurses-6.2$ time make
cd man && make DESTDIR="" RPATH_LIST="/usr/lib" all
make[1]: Entering directory '/mnt/lfs/sources/ncurses-6.2/man'
/bin/sh ./MKterminfo.sh ./terminfo.head ./../include/Caps ./../include/Caps-ncurses ./terminfo.tail >terminfo.5

===== TL;DR =====

real	0m8.336s
user	0m34.254s
sys	0m6.055s

lfs:/mnt/lfs/sources/ncurses-6.2$

```

<br>
### INPUT
```
time make DESTDIR=$LFS TIC_PATH=$(pwd)/build/progs/tic install
echo "INPUT(-lncursesw)" > $LFS/usr/lib/libncurses.so
mv -v $LFS/usr/lib/libncursesw.so.6* $LFS/lib
ln -sfv ../../lib/$(readlink $LFS/usr/lib/libncursesw.so) $LFS/usr/lib/libncursesw.so
cd ..
rm -rf ncurses-6.2

```

### OUTPUT
```
lfs:/mnt/lfs/sources/ncurses-6.2$ time make DESTDIR=$LFS TIC_PATH=$(pwd)/build/progs/tic install
cd man && make DESTDIR="/mnt/lfs" RPATH_LIST="/usr/lib" install
make[1]: Entering directory '/mnt/lfs/sources/ncurses-6.2/man'
/bin/sh ../edit_man.sh normal installing /mnt/lfs/usr/share/man . terminfo.5 *-config.1 ./*.[0-9]*

===== TL;DR =====

real	0m7.050s
user	0m6.211s
sys	0m1.168s

lfs:/mnt/lfs/sources/ncurses-6.2$ echo "INPUT(-lncursesw)" > $LFS/usr/lib/libncurses.so

lfs:/mnt/lfs/sources/ncurses-6.2$ mv -v $LFS/usr/lib/libncursesw.so.6* $LFS/lib
renamed '/mnt/lfs/usr/lib/libncursesw.so.6' -> '/mnt/lfs/lib/libncursesw.so.6'
renamed '/mnt/lfs/usr/lib/libncursesw.so.6.2' -> '/mnt/lfs/lib/libncursesw.so.6.2'

lfs:/mnt/lfs/sources/ncurses-6.2$ ln -sfv ../../lib/$(readlink $LFS/usr/lib/libncursesw.so) $LFS/usr/lib/libncursesw.so
'/mnt/lfs/usr/lib/libncursesw.so' -> '../../lib/libncursesw.so.6'

lfs:/mnt/lfs/sources/ncurses-6.2$ cd ..

lfs:/mnt/lfs/sources$ rm -rf ncurses-6.2

lfs:/mnt/lfs/sources$

```

<br>
# Bash-5.0

### INPUT
```
tar xf bash-5.0.tar.gz
cd bash-5.0
patch -Np1 -i ../bash-5.0-upstream_fixes-1.patch
./configure --prefix=/usr                   \
            --build=$(support/config.guess) \
            --host=$LFS_TGT                 \
            --without-bash-malloc

```

### OUTPUT
```
lfs:/mnt/lfs/sources$ tar xf bash-5.0.tar.gz

lfs:/mnt/lfs/sources$ cd bash-5.0

lfs:/mnt/lfs/sources/bash-5.0$ patch -Np1 -i ../bash-5.0-upstream_fixes-1.patch
patching file bashhist.c
patching file bashline.c
patching file builtins/evalstring.c

===== TL;DR =====

patching file tests/varenv.right
patching file variables.c
patching file y.tab.c

lfs:/mnt/lfs/sources/bash-5.0$ ./configure --prefix=/usr                   \
>             --build=$(support/config.guess) \
>             --host=$LFS_TGT                 \
>             --without-bash-malloc

===== TL;DR =====

config.status: creating po/POTFILES
config.status: creating po/Makefile
config.status: executing default commands

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
lfs:/mnt/lfs/sources/bash-5.0$ time make
rm -f mksyntax
gcc -DPROGRAM='"bash"' -DCONF_HOSTTYPE='"x86_64"' -DCONF_OSTYPE='"linux-gnu"' ...
gcc -DPROGRAM='"bash"' -DCONF_HOSTTYPE='"x86_64"' -DCONF_OSTYPE='"linux-gnu"' ...

===== TL;DR =====

real	0m8.294s
user	0m30.051s
sys	0m2.976s

```

<br>
### INPUT
```
time make DESTDIR=$LFS install
mv $LFS/usr/bin/bash $LFS/bin/bash
ln -sv bash $LFS/bin/sh
cd ..
rm -rf bash-5.0

```

### OUTPUT
```
lfs:/mnt/lfs/sources/bash-5.0$ time  make DESTDIR=$LFS install

	  ***********************************************************
	  *                                                         *
	  * GNU bash, version 5.0.11(1)-release (x86_64-lfs-linux-gnu)
	  *                                                         *
	  ***********************************************************
mkdir -p -- /mnt/lfs/usr/share/doc/bash

===== TL;DR =====

setpgid
seq
make[1]: Leaving directory '/mnt/lfs/sources/bash-5.0/examples/loadables'

lfs:/mnt/lfs/sources/bash-5.0$ mv $LFS/usr/bin/bash $LFS/bin/bash

lfs:/mnt/lfs/sources/bash-5.0$ ln -sv bash $LFS/bin/sh
'/mnt/lfs/bin/sh' -> 'bash'

lfs:/mnt/lfs/sources/bash-5.0$ cd ..

lfs:/mnt/lfs/sources$ rm -rf bash-5.0

lfs:/mnt/lfs/sources$ 

```

<br>
# Coreutils-8.32

<br>
### INPUT
```
tar xf coreutils-8.32.tar.xz
cd coreutils-8.32
patch -Np1 -i ../coreutils-8.32-i18n-1.patch
./configure --prefix=/usr                     \
            --host=$LFS_TGT                   \
            --build=$(build-aux/config.guess) \
            --enable-install-program=hostname \
            --enable-no-install-program=kill,uptime

```

### OUTPUT
```
fs:/mnt/lfs/sources$ tar xf coreutils-8.32.tar.xz

lfs:/mnt/lfs/sources$ cd coreutils-8.32

lfs:/mnt/lfs/sources/coreutils-8.32$ patch -Np1 -i ../coreutils-8.32-i18n-1.patch
patching file bootstrap.conf
patching file configure.ac
patching file lib/linebuffer.h

===== TL;DR =====

patching file tests/misc/uniq.pl
patching file tests/pr/pr-tests.pl
patching file tests/unexpand/mb.sh

lfs:/mnt/lfs/sources/coreutils-8.32$ ./configure --prefix=/usr                     \
>             --host=$LFS_TGT                   \
>             --build=$(build-aux/config.guess) \
>             --enable-install-program=hostname \

===== TL;DR =====

config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

lfs:/mnt/lfs/sources/coreutils-8.32$ 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
lfs:/mnt/lfs/sources/bash-5.0$ time make
rm -f mksyntax
gcc -DPROGRAM='"bash"' -DCONF_HOSTTYPE='"x86_64"' -DCONF_OSTYPE='"linux-gnu"' -DCONF_MACHTYPE='"x86_64-lfs-linux-gnu"' -DCONF_VENDOR='"lfs"' -DLOCALEDIR='"/usr/share/locale"' -DPACKAGE='"bash"' -DSHELL -DHAVE_CONFIG_H   -I.  -I. -I./include -I./lib   -g -DCROSS_COMPILING -rdynamic -g -DCROSS_COMPILING -o mksyntax ./mksyntax.c 

===== TL;DR =====

real	0m8.340s
user	0m29.895s
sys	0m3.000s

lfs:/mnt/lfs/sources/bash-5.0$ 

```

<br>
### INPUT
```
time make DESTDIR=$LFS install
mv -v $LFS/usr/bin/{cat,chgrp,chmod,chown,cp,date,dd,df,echo} $LFS/bin
mv -v $LFS/usr/bin/{false,ln,ls,mkdir,mknod,mv,pwd,rm}        $LFS/bin
mv -v $LFS/usr/bin/{rmdir,stty,sync,true,uname}               $LFS/bin
mv -v $LFS/usr/bin/{head,nice,sleep,touch}                    $LFS/bin
mv -v $LFS/usr/bin/chroot                                     $LFS/usr/sbin
mkdir -pv $LFS/usr/share/man/man8
mv -v $LFS/usr/share/man/man1/chroot.1                        $LFS/usr/share/man/man8/chroot.8
sed -i 's/"1"/"8"/'                                           $LFS/usr/share/man/man8/chroot.8
cd ..
rm -rf coreutils-8.32

```

### OUTPUT
```
lfs:/mnt/lfs/sources/bash-5.0$ make DESTDIR=$LFS install

	  ***********************************************************
	  *                                                         *
	  * GNU bash, version 5.0.11(1)-release (x86_64-lfs-linux-gnu)
	  *                                                         *
	  ***********************************************************

===== TL;DR =====

setpgid
seq
make[1]: Leaving directory '/mnt/lfs/sources/bash-5.0/examples/loadables'

lfs:/mnt/lfs/sources/bash-5.0$ mv $LFS/usr/bin/bash $LFS/bin/bash

lfs:/mnt/lfs/sources/bash-5.0$ ln -sv bash $LFS/bin/sh
'/mnt/lfs/bin/sh' -> 'bash'

lfs:/mnt/lfs/sources/bash-5.0$ cd ..

lfs:/mnt/lfs/sources$ rm -rf bash-5.0

lfs:/mnt/lfs/sources$ 

```

<br>
# Diffutils-3.7

<br>
### INPUT
```
tar xf diffutils-3.7.tar.xz
cd diffutils-3.7
./configure --prefix=/usr --host=$LFS_TGT

```

### OUTPUT
```
lfs:/mnt/lfs/sources$ tar xf diffutils-3.7.tar.xz

lfs:/mnt/lfs/sources$ cd diffutils-3.7

lfs:/mnt/lfs/sources/diffutils-3.7$ ./configure --prefix=/usr --host=$LFS_TGT
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for x86_64-lfs-linux-gnu-strip... x86_64-lfs-linux-gnu-strip

===== TL;DR =====

config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile

lfs:/mnt/lfs/sources/diffutils-3.7$ 

```

<br>
### INPUT
```
time make

```

### OUTPUT
```
lfs:/mnt/lfs/sources/diffutils-3.7$ time make
Making all in lib
make[1]: Entering directory '/mnt/lfs/sources/diffutils-3.7/lib'
  GEN      alloca.h

===== TL;DR =====

real	0m3.633s
user	0m9.712s
sys	0m1.544s

lfs:/mnt/lfs/sources/diffutils-3.7$ 

```

<br>
### INPUT
```
time make DESTDIR=$LFS install
cd ..
rm -rf diffutils-3.7

```

### OUTPUT
```
lfs:/mnt/lfs/sources/diffutils-3.7$ time make DESTDIR=$LFS install
Making install in lib
make[1]: Entering directory '/mnt/lfs/sources/diffutils-3.7/lib'
make  install-am

===== TL;DR =====

real	0m0.476s
user	0m0.423s
sys	0m0.080s

lfs:/mnt/lfs/sources/diffutils-3.7$ cd ..

lfs:/mnt/lfs/sources$ rm -rf diffutils-3.7

lfs:/mnt/lfs/sources$ 

```

<br>### INPUT### OUTPUT
===== TL;DR =====

<hr>
<hr>
<hr>


```
su -
```

```
cbkadal@osp:~$ su -
Password:

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

* Create LFS-06.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-07.md)
[NEXT](index.md)
<br>

