---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-06.md)
[NEXT](index.md)

<br>
<img src="pictures/LFS-A39.jpg" width="960">

```
ssh -p 6024 lfs@localhost

```

```
rms46@pamulang1:~$ ssh -p 6024 lfs@localhost
lfs@localhost's password:

===== TL;DR =====

lfs@osp:~$ 

```

```
echo $LFS
cd $LFS/sources/

```

```
lfs:~$ echo $LFS
/mnt/lfs

lfs:~$ cd $LFS/sources/

```

<br>
## Binutils-2.35 - Pass 1

```
tar xf binutils-2.35.tar.xz 
cd binutils-2.35
mkdir -v build
cd       build
../configure --prefix=$LFS/tools       \
             --with-sysroot=$LFS        \
             --target=$LFS_TGT          \
             --disable-nls              \
             --disable-werror

```

```
lfs:/mnt/lfs/sources$ tar xf binutils-2.35.tar.xz 

lfs:/mnt/lfs/sources$ cd binutils-2.35

lfs:/mnt/lfs/sources/binutils-2.35$ mkdir -v build
mkdir: created directory 'build'

lfs:/mnt/lfs/sources/binutils-2.35$ cd       build

lfs:/mnt/lfs/sources/binutils-2.35/build$ ../configure --prefix=$LFS/tools       \
>              --with-sysroot=$LFS        \
>              --target=$LFS_TGT          \
>              --disable-nls              \
>              --disable-werror
checking build system type... x86_64-pc-linux-gnu
checking host system type... x86_64-pc-linux-gnu
checking target system type... x86_64-lfs-linux-gnu

===== TL;DR =====

checking where to find the target windmc... just compiled
checking whether to enable maintainer-specific portions of Makefiles... no
configure: creating ./config.status
config.status: creating Makefile
lfs:/mnt/lfs/sources/binutils-2.35/build$ 

```

```
time make

```

```
lfs:/mnt/lfs/sources/binutils-2.35/build$ time make
make[1]: Entering directory '/mnt/lfs/sources/binutils-2.35/build'
make[1]: Nothing to be done for 'all-target'.

===== TL;DR =====

make[4]: Leaving directory '/mnt/lfs/sources/binutils-2.35/build/ld'
make[3]: Leaving directory '/mnt/lfs/sources/binutils-2.35/build/ld'
make[2]: Leaving directory '/mnt/lfs/sources/binutils-2.35/build/ld'
make[1]: Leaving directory '/mnt/lfs/sources/binutils-2.35/build'

real	0m35.635s
user	1m46.108s
sys	0m18.371s

lfs:/mnt/lfs/sources/binutils-2.35/build$

```

```
time make install
cd ../..
rm -rf binutils-2.35

```

```
lfs:/mnt/lfs/sources/binutils-2.35/build$ time make install
make[1]: Entering directory '/mnt/lfs/sources/binutils-2.35/build'
/bin/sh ../mkinstalldirs /mnt/lfs/tools /mnt/lfs/tools

===== TL;DR =====

make[5]: Leaving directory '/mnt/lfs/sources/binutils-2.35/build/binutils'
make[4]: Leaving directory '/mnt/lfs/sources/binutils-2.35/build/binutils'
make[3]: Leaving directory '/mnt/lfs/sources/binutils-2.35/build/binutils'
make[2]: Leaving directory '/mnt/lfs/sources/binutils-2.35/build/binutils'
make[1]: Leaving directory '/mnt/lfs/sources/binutils-2.35/build'

real	0m0.645s
user	0m1.203s
sys	0m0.255s

lfs:/mnt/lfs/sources/binutils-2.35/build$ cd ../..

lfs:/mnt/lfs/sources$ rm -rf binutils-2.35

lfs:/mnt/lfs/sources$ 

```

<br>
## GCC-10.2.0 - Pass 1

```
tar xf gcc-10.2.0.tar.xz
cd gcc-10.2.0/
tar -xf ../mpfr-4.1.0.tar.xz
mv -v mpfr-4.1.0 mpfr
tar -xf ../gmp-6.2.0.tar.xz
mv -v gmp-6.2.0 gmp
tar -xf ../mpc-1.1.0.tar.gz
mv -v mpc-1.1.0 mpc
case $(uname -m) in
  x86_64)
    sed -e '/m64=/s/lib64/lib/' \
        -i.orig gcc/config/i386/t-linux64
 ;;
esac

```

```
lfs:/mnt/lfs/sources$ tar xf gcc-10.2.0.tar.xz

lfs:/mnt/lfs/sources$ cd gcc-10.2.0/

lfs:/mnt/lfs/sources/gcc-10.2.0$ tar -xf ../mpfr-4.1.0.tar.xz

lfs:/mnt/lfs/sources/gcc-10.2.0$ mv -v mpfr-4.1.0 mpfr
renamed 'mpfr-4.1.0' -> 'mpfr'

lfs:/mnt/lfs/sources/gcc-10.2.0$ tar -xf ../gmp-6.2.0.tar.xz

lfs:/mnt/lfs/sources/gcc-10.2.0$ mv -v gmp-6.2.0 gmp
renamed 'gmp-6.2.0' -> 'gmp'

lfs:/mnt/lfs/sources/gcc-10.2.0$ tar -xf ../mpc-1.1.0.tar.gz

lfs:/mnt/lfs/sources/gcc-10.2.0$ mv -v mpc-1.1.0 mpc
renamed 'mpc-1.1.0' -> 'mpc'

lfs:/mnt/lfs/sources/gcc-10.2.0$ case $(uname -m) in
>   x86_64)
>     sed -e '/m64=/s/lib64/lib/' \
>         -i.orig gcc/config/i386/t-linux64
>  ;;
> esac

lfs:/mnt/lfs/sources/gcc-10.2.0$

```


```
mkdir -v build
cd       build
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
lfs:/mnt/lfs/sources/gcc-10.2.0$ mkdir -v build
mkdir: created directory 'build'

lfs:/mnt/lfs/sources/gcc-10.2.0$ cd       build

lfs:/mnt/lfs/sources/gcc-10.2.0/build$ ../configure                                       \
>     --target=$LFS_TGT                              \
>     --prefix=$LFS/tools                            \
>     --with-glibc-version=2.11                      \

===== TL;DR =====

checking whether to enable maintainer-specific portions of Makefiles... no
configure: creating ./config.status
config.status: creating Makefile

lfs:/mnt/lfs/sources/gcc-10.2.0/build$ 

```

```
time make

```

```
top - 00:23:22 up  1:02,  2 users,  load average: 4.38, 4.95, 3.09
Tasks: 122 total,   5 running, 117 sleeping,   0 stopped,   0 zombie
%Cpu0  :  5.3 us,  2.3 sy,  0.0 ni, 92.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu1  :  1.1 us,  0.2 sy,  0.0 ni, 98.5 id,  0.0 wa,  0.0 hi,  0.2 si,  0.0 st
%Cpu2  : 29.7 us,  2.9 sy,  0.0 ni, 61.7 id,  5.7 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu3  : 67.6 us,  3.7 sy,  0.0 ni, 28.8 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu4  :  0.5 us,  0.3 sy,  0.0 ni, 99.2 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
%Cpu5  :  6.4 us,  2.1 sy,  0.0 ni, 89.1 id,  2.3 wa,  0.0 hi,  0.1 si,  0.0 st
MiB Mem :   7882.1 total,   2676.5 free,   2514.3 used,   2691.4 buff/cache
MiB Swap:   1024.0 total,   1024.0 free,      0.0 used.   5063.0 avail Mem 

```

<br>
<img src="pictures/LFS-A40.jpg" width="960">

```
===== TL;DR =====

make[3]: Leaving directory '/mnt/lfs/sources/gcc-10.2.0/build/x86_64-lfs-linux-gnu/libgcc'
make[2]: Leaving directory '/mnt/lfs/sources/gcc-10.2.0/build/x86_64-lfs-linux-gnu/libgcc'
make[1]: Leaving directory '/mnt/lfs/sources/gcc-10.2.0/build'

real	5m20.700s
user	22m38.252s
sys	1m51.009s

```

```
time make install

```

```
===== TL;DR =====

make[2]: Leaving directory '/mnt/lfs/sources/gcc-10.2.0/build/gcc'
make[1]: Leaving directory '/mnt/lfs/sources/gcc-10.2.0/build'

real	0m3.396s
user	0m3.157s
sys	0m1.625s

lfs:/mnt/lfs/sources/gcc-10.2.0/build$

```

```
cd ..
cat gcc/limitx.h gcc/glimits.h gcc/limity.h > \
  `dirname $($LFS_TGT-gcc -print-libgcc-file-name)`/install-tools/include/limits.h
cd ..
rm -rf gcc-10.2.0

```

```
lfs:/mnt/lfs/sources/gcc-10.2.0/build$ cd ..

lfs:/mnt/lfs/sources/gcc-10.2.0$ cat gcc/limitx.h gcc/glimits.h gcc/limity.h > \
>   `dirname $($LFS_TGT-gcc -print-libgcc-file-name)`/install-tools/include/limits.h
lfs:/mnt/lfs/sources/gcc-10.2.0$ cd ..

lfs:/mnt/lfs/sources$ rm -rf gcc-10.2.0

lfs:/mnt/lfs/sources$ 

```

<br>
## Linux-5.8.3 API Headers



===== TL;DR =====
===== TL;DR =====
===== TL;DR =====
===== TL;DR =====
===== TL;DR =====
===== TL;DR =====
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

* Create LFS-05.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-06.md)
[NEXT](index.md)
<br>

