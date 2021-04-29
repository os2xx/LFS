---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-04.md)
[NEXT](LFS-09.md)

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
cd ../
rm -rfv coreutils-8.32/

```


<br> 
## ###

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
[PREV](LFS-04.md)
[NEXT](LFS-09.md)
<br>

