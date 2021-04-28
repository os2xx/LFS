---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-04.md)
[NEXT](LFS-08.md)

<br>
# LFS: Chapter 5

## Virtual Box Guest LFS-05

* Import LFS-04.ova, rename to LFS-05

<br>
<img src="pictures/LFS-A39.jpg" width="960">

```
ssh -p 6023 lfs@localhost

```

```
echo $LFS
cd $LFS/sources/

```

## Binutils-2.36.1 - Pass 1

```
tar xvf binutils-2.36.1.tar.xz
cd binutils-2.36.1/

```

```
mkdir -v build
cd       build

```

```
../configure --prefix=$LFS/tools       \
             --with-sysroot=$LFS        \
             --target=$LFS_TGT          \
             --disable-nls              \
             --disable-werror

```

```
time make

```

```
time make install

```

```
cd ../../
rm -rvf binutils-2.36.1/

```

<br>
## GCC-10.2.0 - Pass 1

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
    sed -e '/m64=/s/lib64/lib/' \
        -i.orig gcc/config/i386/t-linux64
 ;;
esac

```

```
mkdir -v build
cd       build

```

```
../configure                                       \
    --target=$LFS_TGT                              \
    --prefix=$LFS/tools                            \
    --with-glibc-version=2.11                      \
    --with-sysroot=$LFS                            \
    --with-newlib                                  \
    --without-headers                              \
    --enable-initfini-array                        \
    --disable-nls                                  \
    --disable-shared                               \
    --disable-multilib                             \
    --disable-decimal-float                        \
    --disable-threads                              \
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
time make install

```

```
cd ..
cat gcc/limitx.h gcc/glimits.h gcc/limity.h > \
  `dirname $($LFS_TGT-gcc -print-libgcc-file-name)`/install-tools/include/limits.h

```


```
cd ../
rm -rvf gcc-10.2.0/

```

<br>
## Linux-5.10.17 API Headers

```
tar xvf linux-5.10.17.tar.xz
cd linux-5.10.17/

```

```
make mrproper

```

```
make headers
find usr/include -name '.*' -delete
rm usr/include/Makefile
cp -rv usr/include $LFS/usr

```

```
cd ../
rm -rvf linux-5.10.17/

```

<br>
## Glibc-2.33

```
tar xvf glibc-2.33.tar.xz
cd glibc-2.33/

```

```
case $(uname -m) in
    i?86)   ln -sfv ld-linux.so.2 $LFS/lib/ld-lsb.so.3
    ;;
    x86_64) ln -sfv ../lib/ld-linux-x86-64.so.2 $LFS/lib64
            ln -sfv ../lib/ld-linux-x86-64.so.2 $LFS/lib64/ld-lsb-x86-64.so.3
    ;;
esac

```

```
patch -Np1 -i ../glibc-2.33-fhs-1.patch

```

```
mkdir -v build
cd       build

```

```
../configure                             \
      --prefix=/usr                      \
      --host=$LFS_TGT                    \
      --build=$(../scripts/config.guess) \
      --enable-kernel=3.2                \
      --with-headers=$LFS/usr/include    \
      libc_cv_slibdir=/lib

```

```
time make

```

```
make DESTDIR=$LFS install

```

```
echo 'int main(){}' > dummy.c
$LFS_TGT-gcc dummy.c
readelf -l a.out | grep '/ld-linux'

```

```
rm -v dummy.c a.out

```

```
$LFS/tools/libexec/gcc/$LFS_TGT/10.2.0/install-tools/mkheaders

```


```
cd ../../
rm -rfv glibc-2.33/

```

<br>
## Libstdc++ from GCC-10.2.0, Pass 1


<br>
## ###




```
su -
```

```
poweroff

```

* Back to "pamulang1" host

* Export LFS-05.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-04.md)
[NEXT](LFS-08.md)
<br>

