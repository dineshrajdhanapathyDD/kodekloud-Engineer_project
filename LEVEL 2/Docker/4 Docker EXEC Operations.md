

## Questions:

One of the Nautilus DevOps team members was working to configure services on a `kkloud` container that is running on `App Server 3` in `Stratos Datacenter`. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:


a. Install `apache2` in `kkloud` container using `apt` that is running on `App Server 3` in `Stratos Datacenter`.


b. Configure Apache to listen on port `6200` instead of default `http` port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.


c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.



## Solution:  


**1. At first login on app server as per the task -app server 3 &  switch to root user**

```

thor@jump_host ~$ ssh banner@stapp03
The authenticity of host 'stapp03 (172.16.238.12)' can't be established.
ECDSA key fingerprint is SHA256:amZCw79A1gdBS/BmG6MVHwsbgf0ott100167ycksrSQ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.
banner@stapp03's password: BigGr33n
[banner@stapp03 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: BigGr33n
```

**2. Run the Below command to check the existing docker container running**

```

[root@stapp03 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
5aff0c8150ba        ubuntu:18.04        "/bin/bash"         4 minutes ago       Up 4 minutes                            kkloud
```

**3. Login on docker container `ubuntu` & install apache2**

```
[root@stapp03 ~]# docker exec -it kkloud /bin/sh

# apt install apache2 -y


Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  apache2-bin apache2-data apache2-utils file libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libexpat1 libgdbm-compat4 libgdbm5 libicu60 liblua5.2-0 libmagic-mgc libmagic1
  libperl5.26 libxml2 mime-support netbase perl perl-modules-5.26 ssl-cert xz-utils
Suggested packages:
  www-browser apache2-doc apache2-suexec-pristine | apache2-suexec-custom ufw gdbm-l10n perl-doc libterm-readline-gnu-perl | libterm-readline-perl-perl make openssl-blacklist
The following NEW packages will be installed:
  apache2 apache2-bin apache2-data apache2-utils file libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libexpat1 libgdbm-compat4 libgdbm5 libicu60 liblua5.2-0 libmagic-mgc
  libmagic1 libperl5.26 libxml2 mime-support netbase perl perl-modules-5.26 ssl-cert xz-utils
0 upgraded, 24 newly installed, 0 to remove and 0 not upgraded.
Need to get 17.5 MB of archives.
After this operation, 88.7 MB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 perl-modules-5.26 all 5.26.1-6ubuntu0.7 [2764 kB]
Get:2 http://archive.ubuntu.com/ubuntu bionic/main amd64 libgdbm5 amd64 1.14.1-6 [26.0 kB]
Get:3 http://archive.ubuntu.com/ubuntu bionic/main amd64 libgdbm-compat4 amd64 1.14.1-6 [6084 B]
Get:4 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libperl5.26 amd64 5.26.1-6ubuntu0.7 [3532 kB]
Get:5 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 perl amd64 5.26.1-6ubuntu0.7 [201 kB]
Get:6 http://archive.ubuntu.com/ubuntu bionic/main amd64 mime-support all 3.60ubuntu1 [30.1 kB]
Get:7 http://archive.ubuntu.com/ubuntu bionic/main amd64 libapr1 amd64 1.6.3-2 [90.9 kB]
Get:8 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libexpat1 amd64 2.2.5-3ubuntu0.9 [82.8 kB]
Get:9 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libaprutil1 amd64 1.6.1-2ubuntu0.1 [84.6 kB]
Get:10 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libaprutil1-dbd-sqlite3 amd64 1.6.1-2ubuntu0.1 [10.6 kB]
Get:11 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libaprutil1-ldap amd64 1.6.1-2ubuntu0.1 [8752 B]
Get:12 http://archive.ubuntu.com/ubuntu bionic/main amd64 liblua5.2-0 amd64 5.2.4-1.1build1 [108 kB]
Get:13 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libicu60 amd64 60.2-3ubuntu3.2 [8050 kB]
Get:14 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libxml2 amd64 2.9.4+dfsg1-6.1ubuntu1.9 [663 kB]
Get:15 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 apache2-bin amd64 2.4.29-1ubuntu4.27 [1071 kB]
Get:16 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 apache2-utils amd64 2.4.29-1ubuntu4.27 [83.3 kB]
Get:17 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 apache2-data all 2.4.29-1ubuntu4.27 [160 kB]
Get:18 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 apache2 amd64 2.4.29-1ubuntu4.27 [95.1 kB]
Get:19 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libmagic-mgc amd64 1:5.32-2ubuntu0.4 [184 kB]
Get:20 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 libmagic1 amd64 1:5.32-2ubuntu0.4 [68.6 kB]
Get:21 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 file amd64 1:5.32-2ubuntu0.4 [22.1 kB]
Get:22 http://archive.ubuntu.com/ubuntu bionic/main amd64 netbase all 5.4 [12.7 kB]
Get:23 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 xz-utils amd64 5.2.2-1.3ubuntu0.1 [83.8 kB]
Get:24 http://archive.ubuntu.com/ubuntu bionic/main amd64 ssl-cert all 1.0.39 [17.0 kB]
Fetched 17.5 MB in 4s (4814 kB/s)   
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package perl-modules-5.26.
(Reading database ... 4578 files and directories currently installed.)
Preparing to unpack .../00-perl-modules-5.26_5.26.1-6ubuntu0.7_all.deb ...
Unpacking perl-modules-5.26 (5.26.1-6ubuntu0.7) ...
Selecting previously unselected package libgdbm5:amd64.
Preparing to unpack .../01-libgdbm5_1.14.1-6_amd64.deb ...
Unpacking libgdbm5:amd64 (1.14.1-6) ...
Selecting previously unselected package libgdbm-compat4:amd64.
Preparing to unpack .../02-libgdbm-compat4_1.14.1-6_amd64.deb ...
Unpacking libgdbm-compat4:amd64 (1.14.1-6) ...
Selecting previously unselected package libperl5.26:amd64.
Preparing to unpack .../03-libperl5.26_5.26.1-6ubuntu0.7_amd64.deb ...
Unpacking libperl5.26:amd64 (5.26.1-6ubuntu0.7) ...
Selecting previously unselected package perl.
Preparing to unpack .../04-perl_5.26.1-6ubuntu0.7_amd64.deb ...
Unpacking perl (5.26.1-6ubuntu0.7) ...
Selecting previously unselected package mime-support.
Preparing to unpack .../05-mime-support_3.60ubuntu1_all.deb ...
Unpacking mime-support (3.60ubuntu1) ...
Selecting previously unselected package libapr1:amd64.
Preparing to unpack .../06-libapr1_1.6.3-2_amd64.deb ...
Unpacking libapr1:amd64 (1.6.3-2) ...
Selecting previously unselected package libexpat1:amd64.
Preparing to unpack .../07-libexpat1_2.2.5-3ubuntu0.9_amd64.deb ...
Unpacking libexpat1:amd64 (2.2.5-3ubuntu0.9) ...
Selecting previously unselected package libaprutil1:amd64.
Preparing to unpack .../08-libaprutil1_1.6.1-2ubuntu0.1_amd64.deb ...
Unpacking libaprutil1:amd64 (1.6.1-2ubuntu0.1) ...
Selecting previously unselected package libaprutil1-dbd-sqlite3:amd64.
Preparing to unpack .../09-libaprutil1-dbd-sqlite3_1.6.1-2ubuntu0.1_amd64.deb ...
Unpacking libaprutil1-dbd-sqlite3:amd64 (1.6.1-2ubuntu0.1) ...
Selecting previously unselected package libaprutil1-ldap:amd64.
Preparing to unpack .../10-libaprutil1-ldap_1.6.1-2ubuntu0.1_amd64.deb ...
Unpacking libaprutil1-ldap:amd64 (1.6.1-2ubuntu0.1) ...
Selecting previously unselected package liblua5.2-0:amd64.
Preparing to unpack .../11-liblua5.2-0_5.2.4-1.1build1_amd64.deb ...
Unpacking liblua5.2-0:amd64 (5.2.4-1.1build1) ...
Selecting previously unselected package libicu60:amd64.
Preparing to unpack .../12-libicu60_60.2-3ubuntu3.2_amd64.deb ...
Unpacking libicu60:amd64 (60.2-3ubuntu3.2) ...
Selecting previously unselected package libxml2:amd64.
Preparing to unpack .../13-libxml2_2.9.4+dfsg1-6.1ubuntu1.9_amd64.deb ...
Unpacking libxml2:amd64 (2.9.4+dfsg1-6.1ubuntu1.9) ...
Selecting previously unselected package apache2-bin.
Preparing to unpack .../14-apache2-bin_2.4.29-1ubuntu4.27_amd64.deb ...
Unpacking apache2-bin (2.4.29-1ubuntu4.27) ...
Selecting previously unselected package apache2-utils.
Preparing to unpack .../15-apache2-utils_2.4.29-1ubuntu4.27_amd64.deb ...
Unpacking apache2-utils (2.4.29-1ubuntu4.27) ...
Selecting previously unselected package apache2-data.
Preparing to unpack .../16-apache2-data_2.4.29-1ubuntu4.27_all.deb ...
Unpacking apache2-data (2.4.29-1ubuntu4.27) ...
Selecting previously unselected package apache2.
Preparing to unpack .../17-apache2_2.4.29-1ubuntu4.27_amd64.deb ...
Unpacking apache2 (2.4.29-1ubuntu4.27) ...
Selecting previously unselected package libmagic-mgc.
Preparing to unpack .../18-libmagic-mgc_1%3a5.32-2ubuntu0.4_amd64.deb ...
Unpacking libmagic-mgc (1:5.32-2ubuntu0.4) ...
Selecting previously unselected package libmagic1:amd64.
Preparing to unpack .../19-libmagic1_1%3a5.32-2ubuntu0.4_amd64.deb ...
Unpacking libmagic1:amd64 (1:5.32-2ubuntu0.4) ...
Selecting previously unselected package file.
Preparing to unpack .../20-file_1%3a5.32-2ubuntu0.4_amd64.deb ...
Unpacking file (1:5.32-2ubuntu0.4) ...
Selecting previously unselected package netbase.
Preparing to unpack .../21-netbase_5.4_all.deb ...
Unpacking netbase (5.4) ...
Selecting previously unselected package xz-utils.
Preparing to unpack .../22-xz-utils_5.2.2-1.3ubuntu0.1_amd64.deb ...
Unpacking xz-utils (5.2.2-1.3ubuntu0.1) ...
Selecting previously unselected package ssl-cert.
Preparing to unpack .../23-ssl-cert_1.0.39_all.deb ...
Unpacking ssl-cert (1.0.39) ...
Setting up libapr1:amd64 (1.6.3-2) ...
Setting up libexpat1:amd64 (2.2.5-3ubuntu0.9) ...
Setting up libicu60:amd64 (60.2-3ubuntu3.2) ...
Setting up mime-support (3.60ubuntu1) ...
Setting up apache2-data (2.4.29-1ubuntu4.27) ...
Setting up ssl-cert (1.0.39) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 76.)
debconf: falling back to frontend: Readline
Setting up perl-modules-5.26 (5.26.1-6ubuntu0.7) ...
Setting up libgdbm5:amd64 (1.14.1-6) ...
Setting up libxml2:amd64 (2.9.4+dfsg1-6.1ubuntu1.9) ...
Setting up libmagic-mgc (1:5.32-2ubuntu0.4) ...
Setting up libmagic1:amd64 (1:5.32-2ubuntu0.4) ...
Setting up xz-utils (5.2.2-1.3ubuntu0.1) ...
update-alternatives: using /usr/bin/xz to provide /usr/bin/lzma (lzma) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/lzma.1.gz because associated file /usr/share/man/man1/xz.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/unlzma.1.gz because associated file /usr/share/man/man1/unxz.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzcat.1.gz because associated file /usr/share/man/man1/xzcat.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzmore.1.gz because associated file /usr/share/man/man1/xzmore.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzless.1.gz because associated file /usr/share/man/man1/xzless.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzdiff.1.gz because associated file /usr/share/man/man1/xzdiff.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzcmp.1.gz because associated file /usr/share/man/man1/xzcmp.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzgrep.1.gz because associated file /usr/share/man/man1/xzgrep.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzegrep.1.gz because associated file /usr/share/man/man1/xzegrep.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzfgrep.1.gz because associated file /usr/share/man/man1/xzfgrep.1.gz (of link group lzma) doesn't exist
Setting up libaprutil1:amd64 (1.6.1-2ubuntu0.1) ...
Setting up liblua5.2-0:amd64 (5.2.4-1.1build1) ...
Setting up libgdbm-compat4:amd64 (1.14.1-6) ...
Setting up libaprutil1-ldap:amd64 (1.6.1-2ubuntu0.1) ...
Setting up netbase (5.4) ...
Setting up libaprutil1-dbd-sqlite3:amd64 (1.6.1-2ubuntu0.1) ...
Setting up apache2-utils (2.4.29-1ubuntu4.27) ...
Setting up file (1:5.32-2ubuntu0.4) ...
Setting up libperl5.26:amd64 (5.26.1-6ubuntu0.7) ...
Setting up perl (5.26.1-6ubuntu0.7) ...
Setting up apache2-bin (2.4.29-1ubuntu4.27) ...
Setting up apache2 (2.4.29-1ubuntu4.27) ...
Enabling module mpm_event.
Enabling module authz_core.
Enabling module authz_host.
Enabling module authn_core.
Enabling module auth_basic.
Enabling module access_compat.
Enabling module authn_file.
Enabling module authz_user.
Enabling module alias.
Enabling module dir.
Enabling module autoindex.
Enabling module env.
Enabling module mime.
Enabling module negotiation.
Enabling module setenvif.
Enabling module filter.
Enabling module deflate.
Enabling module status.
Enabling module reqtimeout.
Enabling conf charset.
Enabling conf localized-error-pages.
Enabling conf other-vhosts-access-log.
Enabling conf security.
Enabling conf serve-cgi-bin.
Enabling site 000-default.
invoke-rc.d: could not determine current runlevel
invoke-rc.d: policy-rc.d denied execution of start.
Processing triggers for libc-bin (2.27-3ubuntu1.6) ...
```

**4.Go to the apache2 folder where all configuration files you will find**

```

# cd /etc/apache2
# ls -l
total 80
-rw-r--r-- 1 root root  7224 Mar  8  2023 apache2.conf
drwxr-xr-x 2 root root  4096 Sep 12 10:05 conf-available
drwxr-xr-x 2 root root  4096 Sep 12 10:05 conf-enabled
-rw-r--r-- 1 root root  1782 Feb 23  2021 envvars
-rw-r--r-- 1 root root 31063 Feb 23  2021 magic
drwxr-xr-x 2 root root 12288 Sep 12 10:05 mods-available
drwxr-xr-x 2 root root  4096 Sep 12 10:05 mods-enabled
-rw-r--r-- 1 root root   320 Feb 23  2021 ports.conf
drwxr-xr-x 2 root root  4096 Sep 12 10:05 sites-available
drwxr-xr-x 2 root root  4096 Sep 12 10:05 sites-enabled
```

**5. Since in this Container no Vi editor was installed, hence using sed commands.**

- If you want you cant install vi editor and edit the below config

# sed -i 's/Listen 80/Listen 6200/g' ports.conf                                                                        
# sed -i 's/:80/:6200/g' apache2.conf
# sed -i 's/#ServerName www.example.com/ServerName localhost/g' apache2.conf


**6. Start apache2 service & confirm the running status**

```

# service apache2 start
 * Starting Apache httpd web server apache2                                                                                                                                                   AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.18.0.2. Set the 'ServerName' directive globally to suppress this message
 * 
# service apache2 status
 * apache2 is running
```

**7. Validate the task by Curl**

```

# curl -Ik localhost:6200
HTTP/1.1 200 OK
Date: Tue, 12 Sep 2023 10:13:13 GMT
Server: Apache/2.4.29 (Ubuntu)
Last-Modified: Tue, 12 Sep 2023 10:05:15 GMT
ETag: "2aa6-605269452eaae"
Accept-Ranges: bytes
Content-Length: 10918
Vary: Accept-Encoding
Content-Type: text/html
```

**8.  Click on `Finish` & `Confirm` to complete the task successfully.**