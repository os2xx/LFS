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

# LFS: UPDATE the SYSTEM

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
ECDSA key fingerprint is SHA256:XYZZYXYZZYXYZZYXYZZYXYZZYXYZZYXYZZYXYZZYXYZ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

Warning: Permanently added '[localhost]:6022' (ECDSA) to the list of known hosts.
Linux osp 4.19.0-16-amd64 #1 SMP Debian 4.19.181-1 (2021-03-19) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Mon Apr 19 08:45:18 2021 from 10.0.2.2

cbkadal:~$

```

<br>
## File .bash_profile

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
echo "syntax off" > .vimrc
source ~/.bash_profile
echo   "Checking LFS=$LFS"
ls -al $LFS/

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

cbkadal@osp:~$ echo "syntax off" > .vimrc

cbkadal@osp:~$ source ~/.bash_profile

cbkadal:~$ echo   "Checking LFS=$LFS"
Checking LFS=/mnt/lfs
cbkadal:~$ ls -al $LFS/
total 24
drwxr-xr-x 3 cbkadal cbkadal  4096 Feb 15 19:00 .
drwxr-xr-x 3 root    root     4096 Apr 19 10:10 ..
drwx------ 2 cbkadal cbkadal 16384 Apr 19 10:19 lost+found

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
echo "syntax off" > .vimrc
source ~/.bash_profile
echo   "Checking LFS=$LFS"
ls -al $LFS/

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

root@osp:~# echo "syntax off" > .vimrc

root@osp:~# source ~/.bash_profile

root:~# echo   "Checking LFS=$LFS"
Checking LFS=/mnt/lfs

root:~# ls -al $LFS/
total 24
drwxr-xr-x 3 cbkadal cbkadal  4096 Feb 15 19:00 .
drwxr-xr-x 3 root    root     4096 Apr 19 10:10 ..
drwx------ 2 cbkadal cbkadal 16384 Apr 19 10:19 lost+found

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
Hit:1 http://deb.debian.org/debian buster InRelease
Hit:2 http://deb.debian.org/debian buster-updates InRelease
Hit:3 http://security.debian.org/debian-security buster/updates InRelease
Hit:4 https://dl.yarnpkg.com/debian stable InRelease
Reading package lists... Done

root:~# apt-get dist-upgrade -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

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
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

root:~# apt-get autoclean -y </dev/null
Reading package lists... Done
Building dependency tree       
Reading state information... Done

root:~# apt-get clean -y </dev/null

root:~# cd /bin

root:/bin# ls -al sh
lrwxrwxrwx 1 root root 4 Feb 15 19:11 sh -> dash

root:/bin# rm sh

root:/bin# ln -s bash sh

root:/bin# ls -al sh
lrwxrwxrwx 1 root root 4 Apr 19 14:36 sh -> bash

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
### INPUT-08
```
poweroff

```

### OUTPUT-08
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

