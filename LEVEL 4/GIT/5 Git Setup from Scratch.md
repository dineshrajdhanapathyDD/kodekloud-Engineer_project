

## Question
Some new developers have joined  `xFusionCorp Industries`  and have been assigned  `Nautilus`  project. They are going to start development on a new application, and some pre-requisites have been shared with the DevOps team to proceed with. Please note that all tasks need to be performed on  `storage server`  in  `Stratos`  DC.  
  

  

a. Install git, set up any values for user.email and user.name globally and create a bare repository  `/opt/games.git`.  
  

b. There is an  `update`  hook (to block direct pushes to the  `master`  branch) under  `/tmp`  on  `storage server`  itself; use the same to block direct pushes to the  `master`  branch in  `/opt/games.git repo`.  
  

c. Clone  `/opt/games.git`  repo in  `/usr/src/kodekloudrepos/games`  directory.  
  

d. Create a new branch named  `xfusioncorp_games`  in repo that you cloned under  `/usr/src/kodekloudrepos`  directory.  
  

e. There is a  `readme.md`  file in  `/tmp`  directory on  `storage server`  itself; copy that to the repo,  `add/commit`  in the new branch you just created, and finally push your branch to the origin.  
  

f. Also create  `master`  branch from your branch and remember you should not be able to push to the  `master`  directly as per the hook you have set up.

## Solution

**1. At first login on to the storage server & Switch to the root user**

```

thor@jump_host ~$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:EdOwgG3xsI2g1V7gvC+1FGHnJQvaAGzUakjIQFnXnCE.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
natasha@ststor01's password: Bl@kW

[natasha@ststor01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for natasha: Bl@kW
[root@ststor01 ~]# 
```

**2. Install Git package**

```

[root@ststor01 ~]# yum install -y git
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                               14 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - AppStream                                               29 MB/s |  34 MB     00:01    
CentOS Stream 8 - BaseOS                                                  11 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                  33 MB/s |  52 MB     00:01    
CentOS Stream 8 - Extras                                                 9.4 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                  11 kB/s | 3.0 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                           1.5 MB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                        9.1 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                495 kB/s |  99 kB     00:00    
Dependencies resolved.
=========================================================================================================
 Package                   Arch      Version                               Repository               Size
=========================================================================================================
Installing:
 git                       x86_64    2.39.3-1.el8_8                        ubi-8-appstream-rpms    104 k
Installing dependencies:
 emacs-filesystem          noarch    1:26.1-11.el8                         baseos                   70 k
 git-core                  x86_64    2.39.3-1.el8_8                        ubi-8-appstream-rpms     11 M
 git-core-doc              noarch    2.39.3-1.el8_8                        ubi-8-appstream-rpms    3.0 M
 groff-base                x86_64    1.22.3-18.el8                         baseos                  1.0 M
 less                      x86_64    530-1.el8                             baseos                  164 k
 ncurses                   x86_64    6.1-9.20180224.el8                    baseos                  387 k
 perl-Carp                 noarch    1.42-396.el8                          baseos                   30 k
 perl-Encode               x86_64    4:2.97-3.el8                          baseos                  1.5 M
 perl-Errno                x86_64    1.28-422.el8                          baseos                   76 k
 perl-Error                noarch    1:0.17025-2.el8                       appstream                46 k
 perl-Exporter             noarch    5.72-396.el8                          baseos                   34 k
 perl-File-Path            noarch    2.15-2.el8                            baseos                   38 k
 perl-File-Temp            noarch    0.230.600-1.el8                       baseos                   63 k
 perl-Getopt-Long          noarch    1:2.50-4.el8                          baseos                   63 k
 perl-Git                  noarch    2.39.3-1.el8_8                        ubi-8-appstream-rpms     79 k
 perl-HTTP-Tiny            noarch    0.074-1.el8                           baseos                   58 k
 perl-IO                   x86_64    1.38-422.el8                          baseos                  142 k
 perl-MIME-Base64          x86_64    3.15-396.el8                          baseos                   31 k
 perl-PathTools            x86_64    3.74-1.el8                            baseos                   90 k
 perl-Pod-Escapes          noarch    1:1.07-395.el8                        baseos                   20 k
 perl-Pod-Perldoc          noarch    3.28-396.el8                          baseos                   86 k
 perl-Pod-Simple           noarch    1:3.35-395.el8                        baseos                  213 k
 perl-Pod-Usage            noarch    4:1.69-395.el8                        baseos                   34 k
 perl-Scalar-List-Utils    x86_64    3:1.49-2.el8                          baseos                   68 k
 perl-Socket               x86_64    4:2.027-3.el8                         baseos                   59 k
 perl-Storable             x86_64    1:3.11-3.el8                          baseos                   98 k
 perl-Term-ANSIColor       noarch    4.06-396.el8                          baseos                   46 k
 perl-Term-Cap             noarch    1.17-395.el8                          baseos                   23 k
 perl-TermReadKey          x86_64    2.37-7.el8                            appstream                40 k
 perl-Text-ParseWords      noarch    3.30-395.el8                          baseos                   18 k
 perl-Text-Tabs+Wrap       noarch    2013.0523-395.el8                     baseos                   24 k
 perl-Time-Local           noarch    1:1.280-1.el8                         baseos                   34 k
 perl-Unicode-Normalize    x86_64    1.25-396.el8                          baseos                   82 k
 perl-constant             noarch    1.33-396.el8                          baseos                   25 k
 perl-interpreter          x86_64    4:5.26.3-422.el8                      baseos                  6.3 M
 perl-libs                 x86_64    4:5.26.3-422.el8                      baseos                  1.6 M
 perl-macros               x86_64    4:5.26.3-422.el8                      baseos                   73 k
 perl-parent               noarch    1:0.237-1.el8                         baseos                   20 k
 perl-podlators            noarch    4.11-1.el8                            baseos                  118 k
 perl-threads              x86_64    1:2.21-2.el8                          baseos                   61 k
 perl-threads-shared       x86_64    1.58-2.el8                            baseos                   48 k
Installing weak dependencies:
 perl-IO-Socket-IP         noarch    0.39-5.el8                            appstream                47 k
 perl-Mozilla-CA           noarch    20160104-7.module_el8+645+9d809f8c    appstream                15 k

Transaction Summary
=========================================================================================================
Install  44 Packages

Total download size: 27 M
Installed size: 82 M
Downloading Packages:
(1/44): perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch.rpm    123 kB/s |  15 kB     00:00    
(2/44): perl-Error-0.17025-2.el8.noarch.rpm                              296 kB/s |  46 kB     00:00    
(3/44): perl-IO-Socket-IP-0.39-5.el8.noarch.rpm                          293 kB/s |  47 kB     00:00    
(4/44): perl-TermReadKey-2.37-7.el8.x86_64.rpm                           435 kB/s |  40 kB     00:00    
(5/44): emacs-filesystem-26.1-11.el8.noarch.rpm                          247 kB/s |  70 kB     00:00    
(6/44): less-530-1.el8.x86_64.rpm                                        558 kB/s | 164 kB     00:00    
(7/44): perl-Carp-1.42-396.el8.noarch.rpm                                532 kB/s |  30 kB     00:00    
(8/44): ncurses-6.1-9.20180224.el8.x86_64.rpm                            2.5 MB/s | 387 kB     00:00    
(9/44): groff-base-1.22.3-18.el8.x86_64.rpm                              2.2 MB/s | 1.0 MB     00:00    
(10/44): perl-Errno-1.28-422.el8.x86_64.rpm                              1.0 MB/s |  76 kB     00:00    
(11/44): perl-Exporter-5.72-396.el8.noarch.rpm                           532 kB/s |  34 kB     00:00    
(12/44): perl-File-Path-2.15-2.el8.noarch.rpm                            626 kB/s |  38 kB     00:00    
(13/44): perl-File-Temp-0.230.600-1.el8.noarch.rpm                       381 kB/s |  63 kB     00:00    
(14/44): perl-Getopt-Long-2.50-4.el8.noarch.rpm                          311 kB/s |  63 kB     00:00    
(15/44): perl-Encode-2.97-3.el8.x86_64.rpm                               3.6 MB/s | 1.5 MB     00:00    
(16/44): perl-HTTP-Tiny-0.074-1.el8.noarch.rpm                           233 kB/s |  58 kB     00:00    
(17/44): perl-MIME-Base64-3.15-396.el8.x86_64.rpm                        202 kB/s |  31 kB     00:00    
(18/44): perl-IO-1.38-422.el8.x86_64.rpm                                 676 kB/s | 142 kB     00:00    
(19/44): perl-PathTools-3.74-1.el8.x86_64.rpm                            742 kB/s |  90 kB     00:00    
(20/44): perl-Pod-Escapes-1.07-395.el8.noarch.rpm                        203 kB/s |  20 kB     00:00    
(21/44): perl-Pod-Perldoc-3.28-396.el8.noarch.rpm                        861 kB/s |  86 kB     00:00    
(22/44): perl-Pod-Usage-1.69-395.el8.noarch.rpm                          674 kB/s |  34 kB     00:00    
(23/44): perl-Scalar-List-Utils-1.49-2.el8.x86_64.rpm                    1.3 MB/s |  68 kB     00:00    
(24/44): perl-Pod-Simple-3.35-395.el8.noarch.rpm                         3.4 MB/s | 213 kB     00:00    
(25/44): perl-Socket-2.027-3.el8.x86_64.rpm                              1.1 MB/s |  59 kB     00:00    
(26/44): perl-Storable-3.11-3.el8.x86_64.rpm                             1.8 MB/s |  98 kB     00:00    
(27/44): perl-Term-ANSIColor-4.06-396.el8.noarch.rpm                     876 kB/s |  46 kB     00:00    
(28/44): perl-Term-Cap-1.17-395.el8.noarch.rpm                           430 kB/s |  23 kB     00:00    
(29/44): perl-Text-ParseWords-3.30-395.el8.noarch.rpm                    350 kB/s |  18 kB     00:00    
(30/44): perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch.rpm                478 kB/s |  24 kB     00:00    
(31/44): perl-Time-Local-1.280-1.el8.noarch.rpm                          524 kB/s |  34 kB     00:00    
(32/44): perl-constant-1.33-396.el8.noarch.rpm                           418 kB/s |  25 kB     00:00    
(33/44): perl-Unicode-Normalize-1.25-396.el8.x86_64.rpm                  1.2 MB/s |  82 kB     00:00    
(34/44): perl-macros-5.26.3-422.el8.x86_64.rpm                           1.3 MB/s |  73 kB     00:00    
(35/44): perl-libs-5.26.3-422.el8.x86_64.rpm                              12 MB/s | 1.6 MB     00:00    
(36/44): perl-parent-0.237-1.el8.noarch.rpm                              277 kB/s |  20 kB     00:00    
(37/44): perl-podlators-4.11-1.el8.noarch.rpm                            1.8 MB/s | 118 kB     00:00    
(38/44): perl-threads-2.21-2.el8.x86_64.rpm                              985 kB/s |  61 kB     00:00    
(39/44): perl-threads-shared-1.58-2.el8.x86_64.rpm                       809 kB/s |  48 kB     00:00    
(40/44): git-2.39.3-1.el8_8.x86_64.rpm                                   1.2 MB/s | 104 kB     00:00    
(41/44): git-core-doc-2.39.3-1.el8_8.noarch.rpm                           22 MB/s | 3.0 MB     00:00    
(42/44): perl-Git-2.39.3-1.el8_8.noarch.rpm                              3.8 MB/s |  79 kB     00:00    
(43/44): git-core-2.39.3-1.el8_8.x86_64.rpm                               36 MB/s |  11 MB     00:00    
(44/44): perl-interpreter-5.26.3-422.el8.x86_64.rpm                      7.1 MB/s | 6.3 MB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                    9.3 MB/s |  27 MB     00:02     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Installing       : ncurses-6.1-9.20180224.el8.x86_64                                              1/44 
  Installing       : less-530-1.el8.x86_64                                                          2/44 
  Installing       : git-core-2.39.3-1.el8_8.x86_64                                                 3/44 
  Installing       : git-core-doc-2.39.3-1.el8_8.noarch                                             4/44 
  Installing       : groff-base-1.22.3-18.el8.x86_64                                                5/44 
  Installing       : perl-Pod-Escapes-1:1.07-395.el8.noarch                                         6/44 
  Installing       : perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch                      7/44 
  Installing       : perl-Time-Local-1:1.280-1.el8.noarch                                           8/44 
  Installing       : perl-IO-Socket-IP-0.39-5.el8.noarch                                            9/44 
  Installing       : perl-Term-ANSIColor-4.06-396.el8.noarch                                       10/44 
  Installing       : perl-Term-Cap-1.17-395.el8.noarch                                             11/44 
  Installing       : perl-File-Temp-0.230.600-1.el8.noarch                                         12/44 
  Installing       : perl-Pod-Simple-1:3.35-395.el8.noarch                                         13/44 
  Installing       : perl-HTTP-Tiny-0.074-1.el8.noarch                                             14/44 
  Installing       : perl-podlators-4.11-1.el8.noarch                                              15/44 
  Installing       : perl-Pod-Perldoc-3.28-396.el8.noarch                                          16/44 
  Installing       : perl-Text-ParseWords-3.30-395.el8.noarch                                      17/44 
  Installing       : perl-Pod-Usage-4:1.69-395.el8.noarch                                          18/44 
  Installing       : perl-MIME-Base64-3.15-396.el8.x86_64                                          19/44 
  Installing       : perl-Storable-1:3.11-3.el8.x86_64                                             20/44 
  Installing       : perl-Getopt-Long-1:2.50-4.el8.noarch                                          21/44 
  Installing       : perl-Errno-1.28-422.el8.x86_64                                                22/44 
  Installing       : perl-Socket-4:2.027-3.el8.x86_64                                              23/44 
  Installing       : perl-Encode-4:2.97-3.el8.x86_64                                               24/44 
  Installing       : perl-Carp-1.42-396.el8.noarch                                                 25/44 
  Installing       : perl-Exporter-5.72-396.el8.noarch                                             26/44 
  Installing       : perl-libs-4:5.26.3-422.el8.x86_64                                             27/44 
  Installing       : perl-Scalar-List-Utils-3:1.49-2.el8.x86_64                                    28/44 
  Installing       : perl-parent-1:0.237-1.el8.noarch                                              29/44 
  Installing       : perl-macros-4:5.26.3-422.el8.x86_64                                           30/44 
  Installing       : perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch                                  31/44 
  Installing       : perl-Unicode-Normalize-1.25-396.el8.x86_64                                    32/44 
  Installing       : perl-File-Path-2.15-2.el8.noarch                                              33/44 
  Installing       : perl-IO-1.38-422.el8.x86_64                                                   34/44 
  Installing       : perl-PathTools-3.74-1.el8.x86_64                                              35/44 
  Installing       : perl-constant-1.33-396.el8.noarch                                             36/44 
  Installing       : perl-threads-1:2.21-2.el8.x86_64                                              37/44 
  Installing       : perl-threads-shared-1.58-2.el8.x86_64                                         38/44 
  Installing       : perl-interpreter-4:5.26.3-422.el8.x86_64                                      39/44 
  Installing       : perl-Error-1:0.17025-2.el8.noarch                                             40/44 
  Installing       : perl-TermReadKey-2.37-7.el8.x86_64                                            41/44 
  Installing       : emacs-filesystem-1:26.1-11.el8.noarch                                         42/44 
  Installing       : git-2.39.3-1.el8_8.x86_64                                                     43/44 
  Installing       : perl-Git-2.39.3-1.el8_8.noarch                                                44/44 
  Running scriptlet: perl-Git-2.39.3-1.el8_8.noarch                                                44/44 
  Verifying        : perl-Error-1:0.17025-2.el8.noarch                                              1/44 
  Verifying        : perl-IO-Socket-IP-0.39-5.el8.noarch                                            2/44 
  Verifying        : perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch                      3/44 
  Verifying        : perl-TermReadKey-2.37-7.el8.x86_64                                             4/44 
  Verifying        : emacs-filesystem-1:26.1-11.el8.noarch                                          5/44 
  Verifying        : groff-base-1.22.3-18.el8.x86_64                                                6/44 
  Verifying        : less-530-1.el8.x86_64                                                          7/44 
  Verifying        : ncurses-6.1-9.20180224.el8.x86_64                                              8/44 
  Verifying        : perl-Carp-1.42-396.el8.noarch                                                  9/44 
  Verifying        : perl-Encode-4:2.97-3.el8.x86_64                                               10/44 
  Verifying        : perl-Errno-1.28-422.el8.x86_64                                                11/44 
  Verifying        : perl-Exporter-5.72-396.el8.noarch                                             12/44 
  Verifying        : perl-File-Path-2.15-2.el8.noarch                                              13/44 
  Verifying        : perl-File-Temp-0.230.600-1.el8.noarch                                         14/44 
  Verifying        : perl-Getopt-Long-1:2.50-4.el8.noarch                                          15/44 
  Verifying        : perl-HTTP-Tiny-0.074-1.el8.noarch                                             16/44 
  Verifying        : perl-IO-1.38-422.el8.x86_64                                                   17/44 
  Verifying        : perl-MIME-Base64-3.15-396.el8.x86_64                                          18/44 
  Verifying        : perl-PathTools-3.74-1.el8.x86_64                                              19/44 
  Verifying        : perl-Pod-Escapes-1:1.07-395.el8.noarch                                        20/44 
  Verifying        : perl-Pod-Perldoc-3.28-396.el8.noarch                                          21/44 
  Verifying        : perl-Pod-Simple-1:3.35-395.el8.noarch                                         22/44 
  Verifying        : perl-Pod-Usage-4:1.69-395.el8.noarch                                          23/44 
  Verifying        : perl-Scalar-List-Utils-3:1.49-2.el8.x86_64                                    24/44 
  Verifying        : perl-Socket-4:2.027-3.el8.x86_64                                              25/44 
  Verifying        : perl-Storable-1:3.11-3.el8.x86_64                                             26/44 
  Verifying        : perl-Term-ANSIColor-4.06-396.el8.noarch                                       27/44 
  Verifying        : perl-Term-Cap-1.17-395.el8.noarch                                             28/44 
  Verifying        : perl-Text-ParseWords-3.30-395.el8.noarch                                      29/44 
  Verifying        : perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch                                  30/44 
  Verifying        : perl-Time-Local-1:1.280-1.el8.noarch                                          31/44 
  Verifying        : perl-Unicode-Normalize-1.25-396.el8.x86_64                                    32/44 
  Verifying        : perl-constant-1.33-396.el8.noarch                                             33/44 
  Verifying        : perl-interpreter-4:5.26.3-422.el8.x86_64                                      34/44 
  Verifying        : perl-libs-4:5.26.3-422.el8.x86_64                                             35/44 
  Verifying        : perl-macros-4:5.26.3-422.el8.x86_64                                           36/44 
  Verifying        : perl-parent-1:0.237-1.el8.noarch                                              37/44 
  Verifying        : perl-podlators-4.11-1.el8.noarch                                              38/44 
  Verifying        : perl-threads-1:2.21-2.el8.x86_64                                              39/44 
  Verifying        : perl-threads-shared-1.58-2.el8.x86_64                                         40/44 
  Verifying        : git-2.39.3-1.el8_8.x86_64                                                     41/44 
  Verifying        : git-core-2.39.3-1.el8_8.x86_64                                                42/44 
  Verifying        : git-core-doc-2.39.3-1.el8_8.noarch                                            43/44 
  Verifying        : perl-Git-2.39.3-1.el8_8.noarch                                                44/44 
Installed products updated.

Installed:
  emacs-filesystem-1:26.1-11.el8.noarch                     git-2.39.3-1.el8_8.x86_64                   
  git-core-2.39.3-1.el8_8.x86_64                            git-core-doc-2.39.3-1.el8_8.noarch          
  groff-base-1.22.3-18.el8.x86_64                           less-530-1.el8.x86_64                       
  ncurses-6.1-9.20180224.el8.x86_64                         perl-Carp-1.42-396.el8.noarch               
  perl-Encode-4:2.97-3.el8.x86_64                           perl-Errno-1.28-422.el8.x86_64              
  perl-Error-1:0.17025-2.el8.noarch                         perl-Exporter-5.72-396.el8.noarch           
  perl-File-Path-2.15-2.el8.noarch                          perl-File-Temp-0.230.600-1.el8.noarch       
  perl-Getopt-Long-1:2.50-4.el8.noarch                      perl-Git-2.39.3-1.el8_8.noarch              
  perl-HTTP-Tiny-0.074-1.el8.noarch                         perl-IO-1.38-422.el8.x86_64                 
  perl-IO-Socket-IP-0.39-5.el8.noarch                       perl-MIME-Base64-3.15-396.el8.x86_64        
  perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch perl-PathTools-3.74-1.el8.x86_64            
  perl-Pod-Escapes-1:1.07-395.el8.noarch                    perl-Pod-Perldoc-3.28-396.el8.noarch        
  perl-Pod-Simple-1:3.35-395.el8.noarch                     perl-Pod-Usage-4:1.69-395.el8.noarch        
  perl-Scalar-List-Utils-3:1.49-2.el8.x86_64                perl-Socket-4:2.027-3.el8.x86_64            
  perl-Storable-1:3.11-3.el8.x86_64                         perl-Term-ANSIColor-4.06-396.el8.noarch     
  perl-Term-Cap-1.17-395.el8.noarch                         perl-TermReadKey-2.37-7.el8.x86_64          
  perl-Text-ParseWords-3.30-395.el8.noarch                  perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch
  perl-Time-Local-1:1.280-1.el8.noarch                      perl-Unicode-Normalize-1.25-396.el8.x86_64  
  perl-constant-1.33-396.el8.noarch                         perl-interpreter-4:5.26.3-422.el8.x86_64    
  perl-libs-4:5.26.3-422.el8.x86_64                         perl-macros-4:5.26.3-422.el8.x86_64         
  perl-parent-1:0.237-1.el8.noarch                          perl-podlators-4.11-1.el8.noarch            
  perl-threads-1:2.21-2.el8.x86_64                          perl-threads-shared-1.58-2.el8.x86_64       

Complete!
[root@ststor01 ~]# 
```

**3. Setup git config for the user.email and user.name globally**

```
[root@ststor01 ~]# git config --global --add user.name natasha
[root@ststor01 ~]# git config --global --add user.email natasha@stratos.xfusioncorp.com
[root@ststor01 ~]# 
```

**4. Create a bare repository**

```
[root@ststor01 ~]# git init --bare /opt/games.git
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint: 
hint:   git config --global init.defaultBranch <name>
hint: 
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint: 
hint:   git branch -m <name>
Initialized empty Git repository in /opt/games.git/
[root@ststor01 ~]# 
```

**5. Copy the /tmp/update hook to hooks directory under /opt/games.git**

```
[root@ststor01 ~]# cp /tmp/update  /opt/games.git/hooks/
```

**6. clone git repository under /usr/src/kodekloudrepos/ directory.**

```
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/
[root@ststor01 kodekloudrepos]# git clone  /opt/games.git
Cloning into 'games'...
warning: You appear to have cloned an empty repository.
done.
[root@ststor01 kodekloudrepos]# ls
games
[root@ststor01 kodekloudrepos]# ls -lsd
4 drwxr-xr-x 3 root root 4096 Nov  2 17:42 .
[root@ststor01 kodekloudrepos]# 
```

**7. Navigate to the clone directory /usr/src/kodekloudrepos/games**

```
[root@ststor01 kodekloudrepos]# cd games
[root@ststor01 games]#
```

**8.  Create a new branch as per the task in the repo that you cloned**

```
[root@ststor01 games]# git checkout -b xfusioncorp_games
Switched to a new branch 'xfusioncorp_games'
[root@ststor01 games]# 
```

**9.  There is a readme.md file in /tmp , copy that to repo, add/commit in the new branch and finally push your branch to origin.**

```
[root@ststor01 games]# cp /tmp/readme.md .
[root@ststor01 games]# ls
readme.md
[root@ststor01 games]# git status
On branch xfusioncorp_games

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        readme.md

nothing added to commit but untracked files present (use "git add" to track)
[root@ststor01 games]# ls -lsd
4 drwxr-xr-x 3 root root 4096 Nov  2 17:48 .
[root@ststor01 games]# git add readme.md
[root@ststor01 games]# git status
On branch xfusioncorp_games

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   readme.md

[root@ststor01 games]# git commit -m "Readme file"
[xfusioncorp_games (root-commit) cf0592f] Readme file
 1 file changed, 1 insertion(+)
 create mode 100644 readme.md
[root@ststor01 games]# git push origin xfusioncorp_games
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 250 bytes | 250.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/games.git
 * [new branch]      xfusioncorp_games -> xfusioncorp_games
[root@ststor01 games]#
```

**10. To validate the task switch to master branch and attempt a push to origin .**   -`get an error hook declined`

```
[root@ststor01 games]# ls
readme.md
[root@ststor01 games]#  git checkout -b master
Switched to a new branch 'master'
Your branch is based on 'origin/master', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)
[root@ststor01 games]# git branch
* master
  xfusioncorp_games
[root@ststor01 games]# git push origin master
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: Manual pushes to the master branch is restricted!!
remote: error: hook declined to update refs/heads/master
To /opt/games.git
 ! [remote rejected] master -> master (hook declined)
error: failed to push some refs to '/opt/games.git'
[root@ststor01 games]# 
```
![git 1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/a82bb1b9-995d-436a-be66-23dc806968d8)

**11. Click on `Finish` & `Confirm` to complete the task successful**

![2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/fcfeecec-3649-4346-8262-edaee333f419)