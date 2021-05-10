---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-05.md)
[NEXT](LFS-07-1.md)

<br>
# LFS: Chapter 6

## Virtual Box Guest LFS-06

* Import LFS-05.ova, rename to LFS-06

<br>
<img src="pictures/LFS-A41.jpg" width="960">

```
ssh -p 6023 lfs@localhost

```

```
echo $LFS
cd $LFS/sources/

```

<br>
## M4-1.4.18

```
tar xfv m4-1.4.18.tar.xz
cd m4-1.4.18/

```

```
sed -i 's/IO_ftrylockfile/IO_EOF_SEEN/' lib/*.c
echo "#define _IO_IN_BACKUP 0x100" >> lib/stdio-impl.h

```

```
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(build-aux/config.guess)

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv m4-1.4.18/

```

<br> 
## Ncurses-6.2

```
tar xfv ncurses-6.2.tar.gz
cd ncurses-6.2/

```

```
sed -i s/mawk// configure

```

```
mkdir build
pushd build
  ../configure
  make -C include
  make -C progs tic
popd

```

```
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

```
time make

```

```
time make DESTDIR=$LFS TIC_PATH=$(pwd)/build/progs/tic install
echo "INPUT(-lncursesw)" > $LFS/usr/lib/libncurses.so

```

```
mv -v $LFS/usr/lib/libncursesw.so.6* $LFS/lib

```

```
ln -sfv ../../lib/$(readlink $LFS/usr/lib/libncursesw.so) $LFS/usr/lib/libncursesw.so

```

```
cd ../
rm -rfv ncurses-6.2/

```

<br> 
## Bash-5.1

```
tar xvf bash-5.1.tar.gz
cd bash-5.1/

```

```
./configure --prefix=/usr                   \
            --build=$(support/config.guess) \
            --host=$LFS_TGT                 \
            --without-bash-malloc

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
mv $LFS/usr/bin/bash $LFS/bin/bash

```

```
ln -sv bash $LFS/bin/sh

```

```
cd ../
rm -rfv bash-5.1/

```

<br> 
## Coreutils-8.32

```
tar xvf coreutils-8.32.tar.xz
cd coreutils-8.32/

```

```
./configure --prefix=/usr                     \
            --host=$LFS_TGT                   \
            --build=$(build-aux/config.guess) \
            --enable-install-program=hostname \
            --enable-no-install-program=kill,uptime

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
mv -v $LFS/usr/bin/{cat,chgrp,chmod,chown,cp,date,dd,df,echo} $LFS/bin
mv -v $LFS/usr/bin/{false,ln,ls,mkdir,mknod,mv,pwd,rm}        $LFS/bin
mv -v $LFS/usr/bin/{rmdir,stty,sync,true,uname}               $LFS/bin
mv -v $LFS/usr/bin/{head,nice,sleep,touch}                    $LFS/bin
mv -v $LFS/usr/bin/chroot                                     $LFS/usr/sbin
mkdir -pv $LFS/usr/share/man/man8
mv -v $LFS/usr/share/man/man1/chroot.1                        $LFS/usr/share/man/man8/chroot.8
sed -i 's/"1"/"8"/'                                           $LFS/usr/share/man/man8/chroot.8

```

```
cd ../
rm -rfv coreutils-8.32/

```

<br> 
## Diffutils-3.7

```
tar xfv diffutils-3.7.tar.xz 
cd diffutils-3.7/

```

```
./configure --prefix=/usr --host=$LFS_TGT

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv diffutils-3.7/

```

<br> 
## File-5.39

```
tar xfv file-5.39.tar.gz
cd file-5.39/

```

```
mkdir build
pushd build
  ../configure --disable-bzlib      \
               --disable-libseccomp \
               --disable-xzlib      \
               --disable-zlib
  make
popd

```

```
./configure --prefix=/usr --host=$LFS_TGT --build=$(./config.guess)

```

```
time make FILE_COMPILE=$(pwd)/build/src/file

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv file-5.39/

```

<br> 
## Findutils-4.8.0

```
tar xfv findutils-4.8.0.tar.xz
cd findutils-4.8.0/

```

```
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(build-aux/config.guess)

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
mv -v $LFS/usr/bin/find $LFS/bin
sed -i 's|find:=${BINDIR}|find:=/bin|' $LFS/usr/bin/updatedb

```

```
cd ../
rm -rfv findutils-4.8.0/

```

<br> 
## Gawk-5.1.0

```
tar xfv gawk-5.1.0.tar.xz
cd gawk-5.1.0/

```

```
sed -i 's/extras//' Makefile.in

```

```
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(./config.guess)

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv gawk-5.1.0/

```

<br> 
## Grep-3.6

```
tar xvf grep-3.6.tar.xz
cd grep-3.6/

```

```
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --bindir=/bin

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv grep-3.6/

```

<br> 
## Gzip-1.10

```
tar xvf gzip-1.10.tar.xz
cd gzip-1.10/

```

```
./configure --prefix=/usr --host=$LFS_TGT

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv gzip-1.10/

```

<br> 
## Make-4.3

```
tar xfv make-4.3.tar.gz
cd make-4.3/

```

```
./configure --prefix=/usr   \
            --without-guile \
            --host=$LFS_TGT \
            --build=$(build-aux/config.guess)

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv make-4.3/

```

<br> 
## Patch-2.7.6

```
tar xvf patch-2.7.6.tar.xz
cd patch-2.7.6/

```

```
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --build=$(build-aux/config.guess)

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv patch-2.7.6/

```

<br> 
## Sed-4.8

```
tar xvf sed-4.8.tar.xz
cd sed-4.8/

```

```
./configure --prefix=/usr   \
            --host=$LFS_TGT \
            --bindir=/bin

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv sed-4.8/

```

<br> 
## Tar-1.34

```
tar xvf tar-1.34.tar.xz
cd tar-1.34/

```

```
./configure --prefix=/usr                     \
            --host=$LFS_TGT                   \
            --build=$(build-aux/config.guess) \
            --bindir=/bin

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
cd ../
rm -rfv tar-1.34/

```

<br> 
## Xz-5.2.5

```
tar xfv xz-5.2.5.tar.xz
cd xz-5.2.5/

```

```
./configure --prefix=/usr                     \
            --host=$LFS_TGT                   \
            --build=$(build-aux/config.guess) \
            --disable-static                  \
            --docdir=/usr/share/doc/xz-5.2.5

```

```
time make

```

```
time make DESTDIR=$LFS install

```

```
mv -v $LFS/usr/bin/{lzma,unlzma,lzcat,xz,unxz,xzcat}  $LFS/bin
mv -v $LFS/usr/lib/liblzma.so.*                       $LFS/lib
ln -svf ../../lib/$(readlink $LFS/usr/lib/liblzma.so) $LFS/usr/lib/liblzma.so

```

```
cd ../
rm -rfv xz-5.2.5/

```

<br> 
## Binutils-2.36.1 - Pass 2

```
tar vxf binutils-2.36.1.tar.xz
cd binutils-2.36.1/

```

```
mkdir -v build
cd       build

```

```
../configure                   \
    --prefix=/usr              \
    --build=$(../config.guess) \
    --host=$LFS_TGT            \
    --disable-nls              \
    --enable-shared            \
    --disable-werror           \
    --enable-64-bit-bfd

```

```
time make

```

```
make DESTDIR=$LFS install -j1
install -vm755 libctf/.libs/libctf.so.0.0.0 $LFS/usr/lib

```

```
cd ../../
rm -rfv binutils-2.36.1/

```

<br> 
## GCC-10.2.0 - Pass 2

```
tar xvf gcc-10.2.0.tar.xz
cd gcc-10.2.0/

```

```
tar -xf ../mpfr-4.1.0.tar.xz
mv -v mpfr-4.1.0 mpfr
tar -xf ../gmp-6.2.1.tar.xz
mv -v gmp-6.2.1 gmp
tar -xf ../mpc-1.2.1.tar.gz
mv -v mpc-1.2.1 mpc

```

```
case $(uname -m) in
  x86_64)
    sed -e '/m64=/s/lib64/lib/' -i.orig gcc/config/i386/t-linux64
  ;;
esac

```

```
mkdir -v build
cd       build

```

```
mkdir -pv $LFS_TGT/libgcc
ln -s ../../../libgcc/gthr-posix.h $LFS_TGT/libgcc/gthr-default.h

```

```
../configure                                       \
    --build=$(../config.guess)                     \
    --host=$LFS_TGT                                \
    --prefix=/usr                                  \
    CC_FOR_TARGET=$LFS_TGT-gcc                     \
    --with-build-sysroot=$LFS                      \
    --enable-initfini-array                        \
    --disable-nls                                  \
    --disable-multilib                             \
    --disable-decimal-float                        \
    --disable-libatomic                            \
    --disable-libgomp                              \
    --disable-libquadmath                          \
    --disable-libssp                               \
    --disable-libvtv                               \
    --disable-libstdcxx                            \
    --enable-languages=c,c++

```

```
time make

```

```
time make DESTDIR=$LFS install
install -vm755 libctf/.libs/libctf.so.0.0.0 $LFS/usr/lib

```

```
ln -sv gcc $LFS/usr/bin/cc

```

```
cd ../../
rm -rfv gcc-10.2.0/

```

<br>
## Done

```
su -
```

```
poweroff

```

* Back to "pamulang1" host

* Export LFS-06.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-05.md)
[NEXT](LFS-07-1.md)
<br>

