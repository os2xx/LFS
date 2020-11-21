---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-02.md)
[NEXT](index.md)

<img src="pictures/LFS-A35.jpg" width="960">
<br>

```
ssh -p 6024 cbkadal@localhost
```

```
rms46@pamulang1:~$ ssh -p 6024 cbkadal@localhost
The authenticity of host '[localhost]:6024 ([127.0.0.1]:6024)' can't be established.
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
cbkadal@osp:~$ 

```

```
su -
```

```
cbkadal@osp:~$ su -
Password: 
root@osp:~# 
```

```
apt-get update
apt-get dist-upgrade -y
apt-get autoremove --purge -y
apt-get autoclean -y
apt-get clean -y
cd /bin
ls -al sh
rm sh
ln -s bash sh
ls -al sh
```

```
root@osp:~# apt-get update
Hit:1 http://security.debian.org/debian-security buster/updates InRelease
Hit:2 http://deb.debian.org/debian buster InRelease
Hit:3 http://deb.debian.org/debian buster-updates InRelease
Reading package lists... Done

root@osp:~# apt-get dist-upgrade -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
root@osp:~# apt-get autoremove --purge -y

Reading package lists... Done
Building dependency tree       
Reading state information... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.

root@osp:~# apt-get autoclean -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done
root@osp:~# apt-get clean -y

root@osp:~# cd /bin

root@osp:/bin# ls -al sh
lrwxrwxrwx 1 root root 4 Nov 21 15:28 sh -> dash

root@osp:/bin# rm sh

root@osp:/bin# ln -s bash sh

root@osp:/bin# ls -al sh
lrwxrwxrwx 1 root root 4 Nov 21 15:29 sh -> bash
```

```
DEBS="
apt-file
bison
build-essential
gawk
texinfo
"

apt-get install $DEBS -y


```

```
```




<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-02.md)
[NEXT](index.md)
<br>

