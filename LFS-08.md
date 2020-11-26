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

# Installation of M4

<br>
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
make DESTDIR=$LFS install
cd ..
rm -rf m4-1.4.18

```

### OUTPUT
```
lfs:/mnt/lfs/sources/m4-1.4.18$ make DESTDIR=$LFS install
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

