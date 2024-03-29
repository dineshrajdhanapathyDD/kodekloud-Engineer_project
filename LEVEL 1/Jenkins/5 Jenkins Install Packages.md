

## Questions:
Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

Create a Jenkins job named `install-packages` and configure it to accomplish below given tasks.

Add a string parameter named `PACKAGE`.
Configure it to install a package on the `storage server` in `Stratos Datacenter` provided to the `$PACKAGE` parameter.

Note:

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on (Restart Jenkins when installation is complete and no jobs are running) on plugin installation/update page i.e `update centre`. Also some times Jenkins UI gets stuck when Jenkins service restarts in the back end so in such case please make sure to refresh the UI page.

2. Make sure Jenkins job passes even on repetitive runs as validation may try to build the job multiple times.

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work. 


## Solutions:

**1. connect with jenkins UI**

- Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

**2. create job**

- Create a Jenkins job named `install-packages` and configure it to accomplish below given tasks.

**3. add the plugins ssh, ssh credentials and credentials**

- manage jenkins -> mange plugins-> add ssh, ssh credentials and credentials install the plugins -> restart the jenkins with username and password.

**4. create credentials**

```
username - natasha
password - Bl@kW
id - ststor01
```

**5. configure system in ssh remote host**

```
hostname-> ststor01, Port -> 22, click add creds to natasha 
then select pty checkbox. 
Then click on check connection. 
Once connection is established then go into Jobs
```

**6. configure the created job**

- Add a string parameter named `PACKAGE`. then click save

```
In Build Steps - Select the option Execute shell script on the remote host using ssh
SSH site Box
Command box - Pass the command below and  then save it
echo Bl@kW | sudo -S yum install -y $PACKAGE
```

**7.Build with parameter**
- Run Build with Parameter then in PACKAGE Box
- Pass httpd or nginx
- click "build"

```
console output:
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/install-packages
[SSH] script:
PACKAGE="httpd"

echo Bl@kW | sudo -S yum install -y $PACKAGE

[SSH] executing...

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for natasha: Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - Ap     [===                 ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - AppStream                      15 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - Ap     [===                 ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - Ap     [   ===              ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - Ap 17% [===-                ]  18 MB/s | 5.5 MB     00:01 ETA
CentOS Stream 8 - Ap 47% [=========           ]  19 MB/s |  15 MB     00:00 ETA
CentOS Stream 8 - Ap 80% [================    ]  20 MB/s |  25 MB     00:00 ETA
CentOS Stream 8 - AppStream                      21 MB/s |  31 MB     00:01    
CentOS Stream 8 - Ba     [      ===           ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - Ba     [         ===        ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - BaseOS                         11 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - Ba     [            ===     ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - Ba100% [====================]  13 kB/s | 3.9 kB     00:00 ETA
CentOS Stream 8 - Ba  1% [                    ] 124 kB/s | 563 kB     05:34 ETA
CentOS Stream 8 - Ba 45% [=========           ] 3.7 MB/s |  19 MB     00:06 ETA
CentOS Stream 8 - BaseOS                         33 MB/s |  41 MB     00:01    
CentOS Stream 8 - Ex     [               ===  ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - Ex100% [====================] 9.0 kB/s | 2.9 kB     00:00 ETA
CentOS Stream 8 - Extras                        9.0 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Ex     [===                 ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - Extras common packages         10 kB/s | 3.0 kB     00:00    
CentOS Stream 8 - Ex     [===                 ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - Ex     [   ===              ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - Extras common packages         18 kB/s | 6.8 kB     00:00    
Extra Packages for E     [      ===           ] ---  B/s |   0  B     --:-- ETA
Extra Packages for E     [         ===        ] ---  B/s |   0  B     --:-- ETA
Extra Packages for E     [            ===     ] ---  B/s |   0  B     --:-- ETA
Extra Packages for E  6% [=                   ] 3.4 MB/s | 1.0 MB     00:04 ETA
Extra Packages for E 19% [===-                ] 3.6 MB/s | 3.0 MB     00:03 ETA
Extra Packages for E 29% [=====-              ] 3.7 MB/s | 4.6 MB     00:02 ETA
Extra Packages for E 37% [=======             ] 3.7 MB/s | 5.9 MB     00:02 ETA
Extra Packages for E 43% [========-           ] 3.7 MB/s | 6.8 MB     00:02 ETA
Extra Packages for E 47% [=========           ] 3.6 MB/s | 7.4 MB     00:02 ETA
Extra Packages for E 51% [==========          ] 3.5 MB/s | 8.1 MB     00:02 ETA
Extra Packages for E 55% [===========         ] 3.4 MB/s | 8.7 MB     00:02 ETA
Extra Packages for E 59% [===========-        ] 3.3 MB/s | 9.4 MB     00:01 ETA
Extra Packages for E 63% [============-       ] 3.3 MB/s |  10 MB     00:01 ETA
Extra Packages for E 68% [=============-      ] 3.2 MB/s |  11 MB     00:01 ETA
Extra Packages for E 72% [==============      ] 3.1 MB/s |  11 MB     00:01 ETA
Extra Packages for E 76% [===============     ] 3.1 MB/s |  12 MB     00:01 ETA
Extra Packages for E 79% [===============-    ] 3.0 MB/s |  13 MB     00:01 ETA
Extra Packages for E 84% [================-   ] 2.9 MB/s |  13 MB     00:00 ETA
Extra Packages for E 88% [=================-  ] 2.9 MB/s |  14 MB     00:00 ETA
Extra Packages for E 93% [==================- ] 2.8 MB/s |  15 MB     00:00 ETA
Extra Packages for E 97% [=================== ] 2.8 MB/s |  15 MB     00:00 ETA
Extra Packages for Enterprise Linux 8 - x86_64  2.4 MB/s |  16 MB     00:06    
Extra Packages for E     [               ===  ] ---  B/s |   0  B     --:-- ETA
Extra Packages for E     [===                 ] ---  B/s |   0  B     --:-- ETA
Extra Packages for E     [===                 ] ---  B/s |   0  B     --:-- ETA
Extra Packages for Enterprise Linux Modular 8 - 800 kB/s | 733 kB     00:00    
Red Hat Universal Ba     [   ===              ] ---  B/s |   0  B     --:-- ETA
Red Hat Universal Base Image 8 (RPMs) - BaseOS  3.0 MB/s | 844 kB     00:00    
Red Hat Universal Ba     [      ===           ] ---  B/s |   0  B     --:-- ETA
Red Hat Universal Ba 99% [===================-]  11 MB/s | 3.3 MB     00:00 ETA
Red Hat Universal Base Image 8 (RPMs) - AppStre  10 MB/s | 3.4 MB     00:00    
Red Hat Universal Ba     [         ===        ] ---  B/s |   0  B     --:-- ETA
Red Hat Universal Base Image 8 (RPMs) - CodeRea 842 kB/s | 107 kB     00:00    
Dependencies resolved.
================================================================================
 Package            Arch   Version                   Repository            Size
================================================================================
Installing:
 [1mhttpd             [m x86_64 2.4.37-56.module+el8.8.0+18758+b3a9c8da.6
                                                     ubi-8-appstream-rpms 1.4 M
Installing dependencies:
 [1mapr               [m x86_64 1.6.3-12.el8              appstream            129 k
 [1mapr-util          [m x86_64 1.6.1-9.el8               appstream            106 k
 [1mcentos-logos-httpd[m noarch 85.8-2.el8                appstream             75 k
 [1mhttpd-filesystem  [m noarch 2.4.37-56.module+el8.8.0+18758+b3a9c8da.6
                                                     ubi-8-appstream-rpms  43 k
 [1mhttpd-tools       [m x86_64 2.4.37-56.module+el8.8.0+18758+b3a9c8da.6
                                                     ubi-8-appstream-rpms 110 k
 [1mmailcap           [m noarch 2.1.48-3.el8              baseos                39 k
 [1mmod_http2         [m x86_64 1.15.7-8.module+el8.8.0+18751+b4557bca.3
                                                     ubi-8-appstream-rpms 155 k
Installing weak dependencies:
 [1mapr-util-bdb      [m x86_64 1.6.1-9.el8               appstream             25 k
 [1mapr-util-openssl  [m x86_64 1.6.1-9.el8               appstream             27 k
Enabling module streams:
 httpd                     2.4                                                 

Transaction Summary
================================================================================
Install  10 Packages

Total download size: 2.1 M
Installed size: 5.6 M
Downloading Packages:
CentOS Stream 8 - Ap     [            ===     ] ---  B/s |   0  B     --:-- ETA
CentOS Stream 8 - Ba210% [==========================================] 4.2 kB/s | 1.3 kB     --:-- ETA
(1/10): apr-1.6.3-12  0% [                    ] ---  B/s |   0  B     --:-- ETA
(1/10): apr-util-bdb-1.6.1-9.el8.x86_64.rpm     247 kB/s |  25 kB     00:00    
(2-3/10): apr-util-1  3% [-                   ] 801 kB/s |  81 kB     00:02 ETA
(2/10): apr-util-openssl-1.6.1-9.el8.x86_64.rpm 798 kB/s |  27 kB     00:00    
(3-4/10): apr-util-1 10% [==                  ] 826 kB/s | 235 kB     00:02 ETA
(3/10): apr-util-1.6.1-9.el8.x86_64.rpm         651 kB/s | 106 kB     00:00    
(4-5/10): centos-log 11% [==                  ] 823 kB/s | 242 kB     00:02 ETA
(4/10): apr-1.6.3-12.el8.x86_64.rpm             777 kB/s | 129 kB     00:00    
(5-6/10): mailcap-2. 13% [==-                 ] 831 kB/s | 287 kB     00:02 ETA
(5/10): centos-logos-httpd-85.8-2.el8.noarch.rp 1.1 MB/s |  75 kB     00:00    
(6-7/10): httpd-2.4. 16% [===                 ] 840 kB/s | 362 kB     00:02 ETA
(6/10): httpd-tools-2.4.37-56.module+el8.8.0+18 1.0 MB/s | 110 kB     00:00    
(7-8/10): httpd-2.4. 22% [====-               ] 847 kB/s | 488 kB     00:01 ETA
(7/10): mod_http2-1.15.7-8.module+el8.8.0+18751 5.9 MB/s | 155 kB     00:00    
(8-9/10): httpd-2.4. 36% [=======             ] 902 kB/s | 782 kB     00:01 ETA
(8/10): httpd-filesystem-2.4.37-56.module+el8.8 1.5 MB/s |  43 kB     00:00    
(9-10/10): httpd-2.4 70% [==============      ] 1.0 MB/s | 1.5 MB     00:00 ETA
(9/10): httpd-2.4.37-56.module+el8.8.0+18758+b3 6.5 MB/s | 1.4 MB     00:00    
(10/10): mailcap-2.1 99% [===================-] 1.1 MB/s | 2.1 MB     00:00 ETA
(10/10): mailcap-2.1.48-3.el8.noarch.rpm        163 kB/s |  39 kB     00:00    
--------------------------------------------------------------------------------
Total                                           2.9 MB/s | 2.1 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction

  Preparing        :  [=====                                              ] 1/1
  Preparing        :  [==========                                         ] 1/1
  Preparing        :  [===============                                    ] 1/1
  Preparing        :  [====================                               ] 1/1
  Preparing        :  [=========================                          ] 1/1
  Preparing        :  [==============================                     ] 1/1
  Preparing        :  [===================================                ] 1/1
  Preparing        :  [========================================           ] 1/1
  Preparing        :  [=============================================      ] 1/1
  Preparing        :                                                        1/1 

  Installing       : apr-1.6.3-12.el8.x86_64 [                          ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [==                        ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [=====                     ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [=========                 ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [============              ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [===============           ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [==================        ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [=====================     ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [======================    ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [========================  ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64 [========================= ]  1/10
  Installing       : apr-1.6.3-12.el8.x86_64                               1/10 

  Running scriptlet: apr-1.6.3-12.el8.x86_64                               1/10 

  Installing       : apr-util-bdb-1.6.1-9.el8 [                         ]  2/10
  Installing       : apr-util-bdb-1.6.1-9.el8 [======================== ]  2/10
  Installing       : apr-util-bdb-1.6.1-9.el8.x86_64                       2/10 

  Installing       : apr-util-openssl-1.6.1-9 [                         ]  3/10
  Installing       : apr-util-openssl-1.6.1-9 [======================== ]  3/10
  Installing       : apr-util-openssl-1.6.1-9.el8.x86_64                   3/10 

  Installing       : apr-util-1.6.1-9.el8.x86 [                         ]  4/10
  Installing       : apr-util-1.6.1-9.el8.x86 [===                      ]  4/10
  Installing       : apr-util-1.6.1-9.el8.x86 [=======                  ]  4/10
  Installing       : apr-util-1.6.1-9.el8.x86 [===========              ]  4/10
  Installing       : apr-util-1.6.1-9.el8.x86 [==============           ]  4/10
  Installing       : apr-util-1.6.1-9.el8.x86 [==================       ]  4/10
  Installing       : apr-util-1.6.1-9.el8.x86 [======================   ]  4/10
  Installing       : apr-util-1.6.1-9.el8.x86 [======================== ]  4/10
  Installing       : apr-util-1.6.1-9.el8.x86_64                           4/10 

  Running scriptlet: apr-util-1.6.1-9.el8.x86_64                           4/10 

  Installing       : httpd-tools-2.4.37-56.mo [                         ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [====                     ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [=======                  ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [==========               ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [============             ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [===============          ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [=================        ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [===================      ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [====================     ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [=======================  ]  5/10
  Installing       : httpd-tools-2.4.37-56.mo [======================== ]  5/10
  Installing       : httpd-tools-2.4.37-56.module+el8.8.0+18758+b3a9c8d    5/10 

  Running scriptlet: httpd-filesystem-2.4.37-56.module+el8.8.0+18758+b3    6/10 

  Installing       : httpd-filesystem-2.4.37- [                         ]  6/10
  Installing       : httpd-filesystem-2.4.37- [=                        ]  6/10
  Installing       : httpd-filesystem-2.4.37- [===                      ]  6/10
  Installing       : httpd-filesystem-2.4.37- [==========               ]  6/10
  Installing       : httpd-filesystem-2.4.37- [============             ]  6/10
  Installing       : httpd-filesystem-2.4.37- [==============           ]  6/10
  Installing       : httpd-filesystem-2.4.37- [================         ]  6/10
  Installing       : httpd-filesystem-2.4.37- [==================       ]  6/10
  Installing       : httpd-filesystem-2.4.37- [===================      ]  6/10
  Installing       : httpd-filesystem-2.4.37- [=====================    ]  6/10
  Installing       : httpd-filesystem-2.4.37- [=======================  ]  6/10
  Installing       : httpd-filesystem-2.4.37-56.module+el8.8.0+18758+b3    6/10 

  Installing       : mailcap-2.1.48-3.el8.noa [                         ]  7/10
  Installing       : mailcap-2.1.48-3.el8.noa [===========              ]  7/10
  Installing       : mailcap-2.1.48-3.el8.noa [====================     ]  7/10
  Installing       : mailcap-2.1.48-3.el8.noa [=======================  ]  7/10
  Installing       : mailcap-2.1.48-3.el8.noa [======================== ]  7/10
  Installing       : mailcap-2.1.48-3.el8.noarch                           7/10 

  Installing       : centos-logos-httpd-85.8- [                         ]  8/10
  Installing       : centos-logos-httpd-85.8- [====                     ]  8/10
  Installing       : centos-logos-httpd-85.8- [========                 ]  8/10
  Installing       : centos-logos-httpd-85.8- [============             ]  8/10
  Installing       : centos-logos-httpd-85.8- [================         ]  8/10
  Installing       : centos-logos-httpd-85.8- [====================     ]  8/10
  Installing       : centos-logos-httpd-85.8- [======================== ]  8/10
  Installing       : centos-logos-httpd-85.8-2.el8.noarch                  8/10 

  Installing       : mod_http2-1.15.7-8.modul [                         ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [==                       ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [====                     ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [======                   ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [========                 ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [==========               ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [============             ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [==============           ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [================         ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [==================       ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [====================     ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [======================   ]  9/10
  Installing       : mod_http2-1.15.7-8.modul [======================== ]  9/10
  Installing       : mod_http2-1.15.7-8.module+el8.8.0+18751+b4557bca.3    9/10 

  Installing       : httpd-2.4.37-56.module+e [                         ] 10/10
  Installing       : httpd-2.4.37-56.module+e [=                        ] 10/10
  Installing       : httpd-2.4.37-56.module+e [==                       ] 10/10
  Installing       : httpd-2.4.37-56.module+e [===                      ] 10/10
  Installing       : httpd-2.4.37-56.module+e [====                     ] 10/10
  Installing       : httpd-2.4.37-56.module+e [=====                    ] 10/10
  Installing       : httpd-2.4.37-56.module+e [======                   ] 10/10
  Installing       : httpd-2.4.37-56.module+e [=======                  ] 10/10
  Installing       : httpd-2.4.37-56.module+e [========                 ] 10/10
  Installing       : httpd-2.4.37-56.module+e [=========                ] 10/10
  Installing       : httpd-2.4.37-56.module+e [==========               ] 10/10
  Installing       : httpd-2.4.37-56.module+e [===========              ] 10/10
  Installing       : httpd-2.4.37-56.module+e [============             ] 10/10
  Installing       : httpd-2.4.37-56.module+e [=============            ] 10/10
  Installing       : httpd-2.4.37-56.module+e [==============           ] 10/10
  Installing       : httpd-2.4.37-56.module+e [===============          ] 10/10
  Installing       : httpd-2.4.37-56.module+e [================         ] 10/10
  Installing       : httpd-2.4.37-56.module+e [=================        ] 10/10
  Installing       : httpd-2.4.37-56.module+e [==================       ] 10/10
  Installing       : httpd-2.4.37-56.module+e [===================      ] 10/10
  Installing       : httpd-2.4.37-56.module+e [====================     ] 10/10
  Installing       : httpd-2.4.37-56.module+e [=====================    ] 10/10
  Installing       : httpd-2.4.37-56.module+e [======================   ] 10/10
  Installing       : httpd-2.4.37-56.module+e [=======================  ] 10/10
  Installing       : httpd-2.4.37-56.module+e [======================== ] 10/10
  Installing       : httpd-2.4.37-56.module+el8.8.0+18758+b3a9c8da.6.x8   10/10 

  Running scriptlet: httpd-2.4.37-56.module+el8.8.0+18758+b3a9c8da.6.x8   10/10 

  Verifying        : apr-1.6.3-12.el8.x86_64                               1/10 

  Verifying        : apr-util-1.6.1-9.el8.x86_64                           2/10 

  Verifying        : apr-util-bdb-1.6.1-9.el8.x86_64                       3/10 

  Verifying        : apr-util-openssl-1.6.1-9.el8.x86_64                   4/10 

  Verifying        : centos-logos-httpd-85.8-2.el8.noarch                  5/10 

  Verifying        : mailcap-2.1.48-3.el8.noarch                           6/10 

  Verifying        : httpd-2.4.37-56.module+el8.8.0+18758+b3a9c8da.6.x8    7/10 

  Verifying        : httpd-tools-2.4.37-56.module+el8.8.0+18758+b3a9c8d    8/10 

  Verifying        : mod_http2-1.15.7-8.module+el8.8.0+18751+b4557bca.3    9/10 

  Verifying        : httpd-filesystem-2.4.37-56.module+el8.8.0+18758+b3   10/10 
Installed products updated.

Installed:
  apr-1.6.3-12.el8.x86_64                                                       
  apr-util-1.6.1-9.el8.x86_64                                                   
  apr-util-bdb-1.6.1-9.el8.x86_64                                               
  apr-util-openssl-1.6.1-9.el8.x86_64                                           
  centos-logos-httpd-85.8-2.el8.noarch                                          
  httpd-2.4.37-56.module+el8.8.0+18758+b3a9c8da.6.x86_64                        
  httpd-filesystem-2.4.37-56.module+el8.8.0+18758+b3a9c8da.6.noarch             
  httpd-tools-2.4.37-56.module+el8.8.0+18758+b3a9c8da.6.x86_64                  
  mailcap-2.1.48-3.el8.noarch                                                   
  mod_http2-1.15.7-8.module+el8.8.0+18751+b4557bca.3.x86_64      


Complete!

[SSH] completed
[SSH] exit-status: 0

Finished: SUCCESS
```

**8. Click on `Finish` & `Confirm` to complete the task successful.**









