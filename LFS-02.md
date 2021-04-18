---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-01.md)
[NEXT](LFS-03.md)

# UPDATE the SYSTEM

<br>
## Guest "LFS101-000"

<img src="pictures/L101-004.jpg" width="960">

<br>
## Rename Guest "LFS101-000" to "LFS101-001"

<img src="pictures/L101-006.jpg" width="960">

<br>
## START

<br>
### INPUT-01
```
ssh -p 6022 cbkadal@localhost

```

### OUTPUT-01
```
rms46@pamulang1:~$ ssh -p 6022 cbkadal@localhost
The authenticity of host '[localhost]:6022 ([127.0.0.1]:6022)' can't be established.
ECDSA key fingerprint is SHA256:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

Warning: Permanently added '[localhost]:6024' (ECDSA) to the list of known hosts.
cbkadal@localhost's password: 

Linux osp 4.19.0-12-amd64 #1 SMP Debian 4.19.152-1 (2020-10-18) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
cbkadal:~$ 

```

<br>
### INPUT-02
```
cat > ~/.bash_profile << "EOF"
# Files .bash_profile
alias cl='clear;echo "";'
alias h='history'
alias sss='. ~/.profile'
export EDITOR=/usr/bin/vi
export HISTSIZE=2000
export HISTFILESIZE=2000
export LFS=/mnt/lfs
export PS1='\u:\w\$ '
export MAKEFLAGS='-j6'
EOF
source ~/.bash_profile
echo   "LFS=$LFS"

```

### OUTPUT-02
```
cbkadal@osp:~$ cat > ~/.bash_profile << "EOF"
> # Files .bash_profile
> alias cl='clear;echo "";'
> alias h='history'
> alias sss='. ~/.profile'
> export EDITOR=/usr/bin/vi
> export HISTSIZE=2000
> export HISTFILESIZE=2000
> export LFS=/mnt/lfs
> export PS1='\u:\w\$ '
> export MAKEFLAGS='-j6'
> EOF

cbkadal@osp:~$ source ~/.bash_profile

cbkadal:~$ echo   "LFS=$LFS"
LFS=/mnt/lfs

cbkadal:~$ 

```

<br>
# LFS (Ch. 2) Preparing the LFS Host (=VirtualBox Guest)

### INPUT-03
```
su -

```

### OUTPUT-03
```
cbkadal:~$ su -
Password: 

root:~# 

```

<br>
### INPUT-04
```
cat > ~/.bash_profile << "EOF"
# Files .bash_profile
export EDITOR=/usr/bin/vi
export HISTSIZE=2000
export HISTFILESIZE=2000
export LFS=/mnt/lfs
export PS1='\u:\w\$ '
export MAKEFLAGS='-j6'
EOF
source ~/.bash_profile
echo   "LFS=$LFS"

```

### OUTPUT-04
```
root@osp:~# cat > ~/.bash_profile << "EOF"
> # Files .bash_profile
> export EDITOR=/usr/bin/vi
> export HISTSIZE=2000
> export HISTFILESIZE=2000
> export LFS=/mnt/lfs
> export PS1='\u:\w\$ '
> export MAKEFLAGS='-j6'
> EOF

root@osp:~# source ~/.bash_profile

root:~# echo   "LFS=$LFS"
LFS=/mnt/lfs

root:~#

```

<br>
### INPUT-05
```
apt-get update
apt-get dist-upgrade -y

```

### OUTPUT-05
```
root:~# apt-get update
Get:1 http://security.debian.org/debian-security buster/updates InRelease [65.4 kB]
Get:2 https://dl.yarnpkg.com/debian stable InRelease [17.1 kB]                                               
Get:3 http://security.debian.org/debian-security buster/updates/main amd64 Packages [272 kB]              

===== TL;DR =====

Fetched 52.1 MB in 44s (1,187 kB/s)                                                                          
Reading package lists... Done
N: Repository 'http://deb.debian.org/debian buster InRelease' changed its 'Version' value from '10.8' to '10.9'

root:~# apt-get dist-upgrade -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done

===== TL;DR =====

Need to get 132 MB of archives.
After this operation, 270 MB of additional disk space will be used.
Get:1 http://deb.debian.org/debian buster/main amd64 base-files amd64 10.3+deb10u9 [69.9 kB]
Get:2 http://security.debian.org/debian-security buster/updates/main amd64 libssl-dev amd64 1.1.1d-0+deb10u6 [1,794 kB]

===== TL;DR =====

Unpacking libpam-systemd:amd64 (241-7~deb10u7) over (241-7~deb10u6) ...
Preparing to unpack .../systemd_241-7~deb10u7_amd64.deb ...
Unpacking systemd (241-7~deb10u7) over (241-7~deb10u6) ...

===== TL;DR =====

User sessions running outdated binaries:
 cbkadal @ session #1: bash[709], sshd[691]
 cbkadal @ user manager service: systemd[694]

root:~#

```

<br>
### INPUT-06
```
apt-get autoremove --purge -y </dev/null
apt-get autoclean -y </dev/null
apt-get clean -y </dev/null
cd /bin
ls -al sh
rm sh
ln -s bash sh
ls -al sh
cd

```

### OUTPUT-06

```
root:~# apt-get autoremove --purge -y </dev/null
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages will be REMOVED:
  linux-image-4.19.0-13-amd64*
0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.
After this operation, 270 MB disk space will be freed.
(Reading database ... 59432 files and directories currently installed.)
Removing linux-image-4.19.0-13-amd64 (4.19.160-2) ...
/etc/kernel/postrm.d/initramfs-tools:
update-initramfs: Deleting /boot/initrd.img-4.19.0-13-amd64
/etc/kernel/postrm.d/zz-update-grub:
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.19.0-16-amd64
Found initrd image: /boot/initrd.img-4.19.0-16-amd64
Found linux image: /boot/vmlinuz-4.19.0-14-amd64
Found initrd image: /boot/initrd.img-4.19.0-14-amd64
done
(Reading database ... 55030 files and directories currently installed.)
Purging configuration files for linux-image-4.19.0-13-amd64 (4.19.160-2) ...

root:~# apt-get autoclean -y </dev/null
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Del libnode64 10.23.1~dfsg-1~deb10u1 [5,619 kB]
Del nodejs 10.23.1~dfsg-1~deb10u1 [87.2 kB]
Del python-pygments 2.3.1+dfsg-1 [596 kB]
Del nodejs-doc 10.23.1~dfsg-1~deb10u1 [974 kB]

root:~# apt-get clean -y </dev/null

root:~# cd /bin

root:/bin# ls -al sh
lrwxrwxrwx 1 root root 4 Feb 15 19:11 sh -> dash

root:/bin# rm sh

root:/bin# ln -s bash sh

root:/bin# ls -al sh
lrwxrwxrwx 1 root root 4 Apr 19 00:29 sh -> bash

root:/bin# cd

root:~# 

```

<br>
## Just to be sure

### INPUT-07
```
DEBS="
apt-file
automake
bison
build-essential
gawk
texinfo
parted
"

apt-get install $DEBS -y

```

### OUTPUT-07
```
root:~# DEBS="
> apt-file
> automake
> bison
> build-essential
> gawk
> texinfo
> parted
> "

root:~# apt-get install $DEBS -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done
apt-file is already the newest version (3.2.2).
automake is already the newest version (1:1.16.1-4).
bison is already the newest version (2:3.3.2.dfsg-1).
build-essential is already the newest version (12.6).
gawk is already the newest version (1:4.2.1+dfsg-1).
parted is already the newest version (3.2-25).
texinfo is already the newest version (6.5.0.dfsg.1-4+b1).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

root:~# 

```

<br>
### INPUT
```
poweroff

```

### OUTPUT
```
root:~# poweroff
Connection to localhost closed by remote host.
Connection to localhost closed.

rms46@pamulang1:~$ 

```

* Back to "pamulang1" host

* Export LFS101-001.OVA (backup)

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-01.md)
[NEXT](LFS-03.md)
<br>

