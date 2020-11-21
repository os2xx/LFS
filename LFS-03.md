---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](LFS-02.md)
[NEXT](LFS-04.md)

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
Get:3 http://deb.debian.org/debian buster-updates InRelease [51.9 kB]
Fetched 51.9 kB in 1s (66.4 kB/s)
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
lrwxrwxrwx 1 root root 4 Nov 21 13:29 sh -> dash

root@osp:/bin# rm sh

root@osp:/bin# ln -s bash sh

root@osp:/bin# ls -al sh
lrwxrwxrwx 1 root root 4 Nov 21 16:59 sh -> bash

root@osp:/bin#

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
root@osp:~# DEBS="
> apt-file
> bison
> build-essential
> gawk
> texinfo
> "

root@osp:~# apt-get install $DEBS -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  binutils binutils-common binutils-x86-64-linux-gnu cpp cpp-8 dirmngr dpkg-dev fakeroot g++ g++-8 gcc gcc-8 gnupg
  gnupg-l10n gnupg-utils gpg gpg-agent gpg-wks-client gpg-wks-server gpgconf gpgsm libalgorithm-diff-perl
  libalgorithm-diff-xs-perl libalgorithm-merge-perl libapt-pkg-perl libasan5 libassuan0 libatomic1 libauthen-sasl-perl
  libbinutils libbison-dev libc-dev-bin libc6-dev libcc1-0 libdata-dump-perl libdpkg-perl libencode-locale-perl
  libexporter-tiny-perl libfakeroot libfile-fcntllock-perl libfile-listing-perl libfont-afm-perl libgcc-8-dev libgomp1
  libhtml-form-perl libhtml-format-perl libhtml-parser-perl libhtml-tagset-perl libhtml-tree-perl libhttp-cookies-perl
  libhttp-daemon-perl libhttp-date-perl libhttp-message-perl libhttp-negotiate-perl libio-html-perl
  libio-socket-ssl-perl libisl19 libitm1 libksba8 liblist-moreutils-perl liblsan0 liblwp-mediatypes-perl
  liblwp-protocol-https-perl libmailtools-perl libmpc3 libmpfr6 libmpx2 libnet-http-perl libnet-smtp-ssl-perl
  libnet-ssleay-perl libnpth0 libquadmath0 libregexp-assemble-perl libsigsegv2 libstdc++-8-dev libtext-unidecode-perl
  libtimedate-perl libtry-tiny-perl libtsan0 libubsan1 liburi-perl libwww-perl libwww-robotrules-perl
  libxml-libxml-perl libxml-namespacesupport-perl libxml-parser-perl libxml-sax-base-perl libxml-sax-expat-perl
  libxml-sax-perl linux-libc-dev m4 make manpages-dev patch perl-openssl-defaults pinentry-curses tex-common
Suggested packages:
  binutils-doc bison-doc cpp-doc gcc-8-locales dbus-user-session pinentry-gnome3 tor debian-keyring g++-multilib
  g++-8-multilib gcc-8-doc libstdc++6-8-dbg gawk-doc gcc-multilib autoconf automake libtool flex gdb gcc-doc
  gcc-8-multilib libgcc1-dbg libgomp1-dbg libitm1-dbg libatomic1-dbg libasan5-dbg liblsan0-dbg libtsan0-dbg
  libubsan1-dbg libmpx2-dbg libquadmath0-dbg parcimonie xloadimage scdaemon libdigest-hmac-perl libgssapi-perl
  glibc-doc git bzr libcrypt-ssleay-perl libstdc++-8-doc libauthen-ntlm-perl libxml-sax-expatxs-perl m4-doc make-doc
  ed diffutils-doc pinentry-doc debhelper texlive-base texlive-latex-base texlive-generic-recommended
  texinfo-doc-nonfree texlive-fonts-recommended
The following NEW packages will be installed:
  apt-file binutils binutils-common binutils-x86-64-linux-gnu bison build-essential cpp cpp-8 dirmngr dpkg-dev
  fakeroot g++ g++-8 gawk gcc gcc-8 gnupg gnupg-l10n gnupg-utils gpg gpg-agent gpg-wks-client gpg-wks-server gpgconf
  gpgsm libalgorithm-diff-perl libalgorithm-diff-xs-perl libalgorithm-merge-perl libapt-pkg-perl libasan5 libassuan0
  libatomic1 libauthen-sasl-perl libbinutils libbison-dev libc-dev-bin libc6-dev libcc1-0 libdata-dump-perl
  libdpkg-perl libencode-locale-perl libexporter-tiny-perl libfakeroot libfile-fcntllock-perl libfile-listing-perl
  libfont-afm-perl libgcc-8-dev libgomp1 libhtml-form-perl libhtml-format-perl libhtml-parser-perl libhtml-tagset-perl
  libhtml-tree-perl libhttp-cookies-perl libhttp-daemon-perl libhttp-date-perl libhttp-message-perl
  libhttp-negotiate-perl libio-html-perl libio-socket-ssl-perl libisl19 libitm1 libksba8 liblist-moreutils-perl
  liblsan0 liblwp-mediatypes-perl liblwp-protocol-https-perl libmailtools-perl libmpc3 libmpfr6 libmpx2
  libnet-http-perl libnet-smtp-ssl-perl libnet-ssleay-perl libnpth0 libquadmath0 libregexp-assemble-perl libsigsegv2
  libstdc++-8-dev libtext-unidecode-perl libtimedate-perl libtry-tiny-perl libtsan0 libubsan1 liburi-perl libwww-perl
  libwww-robotrules-perl libxml-libxml-perl libxml-namespacesupport-perl libxml-parser-perl libxml-sax-base-perl
  libxml-sax-expat-perl libxml-sax-perl linux-libc-dev m4 make manpages-dev patch perl-openssl-defaults
  pinentry-curses tex-common texinfo
0 upgraded, 102 newly installed, 0 to remove and 0 not upgraded.
Need to get 63.1 MB of archives.
After this operation, 224 MB of additional disk space will be used.
Get:1 http://security.debian.org/debian-security buster/updates/main amd64 linux-libc-dev amd64 4.19.152-1 [1,402 kB]
Get:2 http://deb.debian.org/debian buster/main amd64 libmpfr6 amd64 4.0.2-1 [775 kB]
Get:3 http://deb.debian.org/debian buster/main amd64 libsigsegv2 amd64 2.12-2 [32.8 kB]
Get:4 http://deb.debian.org/debian buster/main amd64 gawk amd64 1:4.2.1+dfsg-1 [660 kB]

===== TL;DR =====

Get:100 http://deb.debian.org/debian buster/main amd64 libxml-sax-expat-perl all 0.51-1 [12.0 kB]                      
Get:101 http://deb.debian.org/debian buster/main amd64 manpages-dev all 4.16-2 [2,232 kB]                              
Get:102 http://deb.debian.org/debian buster/main amd64 texinfo amd64 6.5.0.dfsg.1-4+b1 [1,431 kB]                      
Fetched 63.1 MB in 31s (2,055 kB/s)                                                                                    
Extracting templates from packages: 100%
Selecting previously unselected package libmpfr6:amd64.
(Reading database ... 31820 files and directories currently installed.)
Preparing to unpack .../libmpfr6_4.0.2-1_amd64.deb ...
Unpacking libmpfr6:amd64 (4.0.2-1) ...

===== TL;DR =====

Setting up liblwp-protocol-https-perl (6.07-2) ...
Setting up libwww-perl (6.36-2) ...
Setting up libxml-parser-perl (2.44-4) ...
Setting up libxml-sax-expat-perl (0.51-1) ...
update-perl-sax-parsers: Registering Perl SAX parser XML::SAX::Expat with priority 50...
update-perl-sax-parsers: Updating overall Perl SAX parser modules info file...
Replacing config file /etc/perl/XML/SAX/ParserDetails.ini with new version
Processing triggers for libc-bin (2.28-10) ...
Processing triggers for man-db (2.8.5-2) ...
root@osp:~# 

```


```
cat > version-check.sh << "EOF"
#!/bin/bash
# Simple script to list version numbers of critical development tools
# Copyright 1999-2020 Gerard Beekmans; 
# Permission is hereby granted, free of charge, to any person obtaining
# a copy of this software and associated documentation files (the 
# "Software"), to deal in the Software without restriction, including 
# without limitation the rights to use, copy, modify, merge, publish, 
# distribute, sublicense, and/or sell copies of the Software, and to 
# permit persons to whom the Software is furnished to do so, subject 
# to the following conditions:
# The above copyright notice and this permission notice shall be 
# included in all copies or substantial portions of the Software.

export LC_ALL=C
bash --version | head -n1 | cut -d" " -f2-4
MYSH=$(readlink -f /bin/sh)
echo "/bin/sh -> $MYSH"
echo $MYSH | grep -q bash || echo "ERROR: /bin/sh does not point to bash"
unset MYSH

echo -n "Binutils: "; ld --version | head -n1 | cut -d" " -f3-
bison --version | head -n1

if [ -h /usr/bin/yacc ]; then
  echo "/usr/bin/yacc -> `readlink -f /usr/bin/yacc`";
elif [ -x /usr/bin/yacc ]; then
  echo yacc is `/usr/bin/yacc --version | head -n1`
else
  echo "yacc not found" 
fi

bzip2 --version 2>&1 < /dev/null | head -n1 | cut -d" " -f1,6-
echo -n "Coreutils: "; chown --version | head -n1 | cut -d")" -f2
diff --version | head -n1
find --version | head -n1
gawk --version | head -n1

if [ -h /usr/bin/awk ]; then
  echo "/usr/bin/awk -> `readlink -f /usr/bin/awk`";
elif [ -x /usr/bin/awk ]; then
  echo awk is `/usr/bin/awk --version | head -n1`
else 
  echo "awk not found" 
fi

gcc --version | head -n1
g++ --version | head -n1
ldd --version | head -n1 | cut -d" " -f2-  # glibc version
grep --version | head -n1
gzip --version | head -n1
cat /proc/version
m4 --version | head -n1
make --version | head -n1
patch --version | head -n1
echo Perl `perl -V:version`
python3 --version
sed --version | head -n1
tar --version | head -n1
makeinfo --version | head -n1  # texinfo version
xz --version | head -n1

echo 'int main(){}' > dummy.c && g++ -o dummy dummy.c
if [ -x dummy ]
  then echo "g++ compilation OK";
  else echo "g++ compilation failed"; fi
rm -f dummy.c dummy
EOF


bash version-check.sh

```

```
cbkadal@osp:/tmp$ cat > version-check.sh << "EOF"
> #!/bin/bash
> # Simple script to list version numbers of critical development tools
> # Copyright 1999-2020 Gerard Beekmans; 

===== TL;DR =====

> rm -f dummy.c dummy
> EOF

cbkadal@osp:/tmp$ bash version-check.sh
bash, version 5.0.3(1)-release
/bin/sh -> /usr/bin/bash
Binutils: (GNU Binutils for Debian) 2.31.1
bison (GNU Bison) 3.3.2
/usr/bin/yacc -> /usr/bin/bison.yacc
bzip2,  Version 1.0.6, 6-Sept-2010.
Coreutils:  8.30
diff (GNU diffutils) 3.7
find (GNU findutils) 4.6.0.225-235f
GNU Awk 4.2.1, API: 2.0 (GNU MPFR 4.0.2, GNU MP 6.1.2)
/usr/bin/awk -> /usr/bin/gawk
gcc (Debian 8.3.0-6) 8.3.0
g++ (Debian 8.3.0-6) 8.3.0
(Debian GLIBC 2.28-10) 2.28
grep (GNU grep) 3.3
gzip 1.9
Linux version 4.19.0-12-amd64 (debian-kernel@lists.debian.org) (gcc version 8.3.0 (Debian 8.3.0-6)) #1 SMP Debian 4.19.152-1 (2020-10-18)
m4 (GNU M4) 1.4.18
GNU Make 4.2.1
GNU patch 2.7.6
Perl version='5.28.1';
Python 3.7.3
sed (GNU sed) 4.7
tar (GNU tar) 1.30
texi2any (GNU texinfo) 6.5
xz (XZ Utils) 5.2.4
g++ compilation OK
cbkadal@osp:/tmp$

```

```
shutdown -h now
```

```
root@osp:~# 
root@osp:~# shutdown -h now
Connection to localhost closed by remote host.
Connection to localhost closed.
rms46@pamulang1:~$
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
[NEXT](LFS-04.md)
<br>

