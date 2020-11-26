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


<br>### INPUT### OUTPUT
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

