

## Questions:

The DevOps team of xFusionCorp Industries is planning to setup some CI/CD pipelines. After several meetings they have decided to use Jenkins server. So, we need to setup a `Jenkins Server` as soon as possible. Please complete the task as per requirements mentioned below:

1. Install `jenkins` on jenkins server using `yum` utility only, and start its `service`. You might face timeout issue while starting the Jenkins service, please refer this link for help.

2. Jenkin's admin user name should be `theadmin`, password should be `Adm!n321`, full name should be `Yousuf` and email should be `yousuf@jenkins.stratos.xfusioncorp.com`.

Note:

1. For this task ssh into `jenkins` server using user `root` and password `S3curePass` from `jump host`.

2. To complete the `jenkins installation` after installing packages and after starting the jenkins service, click on the `Jenkins` button on the top bar.


## Solution:  

**1. Login on the jenkins server as a root**

```
thor@jump_host ~$ ssh root@jenkins
root@jenkins's password: S3curePass

[root@jenkins ~]# sudo yum -y install wget

[root@jenkins ~]# sudo wget -O /etc/yum.repos.d/jenkins.repo \
     https://pkg.jenkins.io/redhat-stable/jenkins.repo
--2023-08-08 07:57:57--  https://pkg.jenkins.io/redhat-stable/jenkins.repo
Resolving pkg.jenkins.io (pkg.jenkins.io)... 151.101.2.133, 151.101.66.133, 151.101.130.133, ...
Connecting to pkg.jenkins.io (pkg.jenkins.io)|151.101.2.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 85
Saving to: ‘/etc/yum.repos.d/jenkins.repo’

/etc/yum.repos.d/jenkins.r 100%[=====================================>]      85  --.-KB/s    in 0s      

2023-08-08 07:57:57 (7.92 MB/s) - ‘/etc/yum.repos.d/jenkins.repo’ saved [85/85]

[root@jenkins ~]# sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

**2. Add required dependencies for the jenkins package**

```
[root@jenkins ~]# sudo dnf upgrade
Failed loading plugin "subscription-manager": cannot import name 'which'
Failed loading plugin "upload-profile": cannot import name 'which'
Failed loading plugin "product-id": cannot import name 'which'
Jenkins-stable                                                            87 kB/s |  27 kB     00:00    
Dependencies resolved.
Nothing to do.
Complete!
[root@jenkins ~]# sudo dnf install java-17-openjdk
Failed loading plugin "subscription-manager": cannot import name 'which'
Failed loading plugin "upload-profile": cannot import name 'which'
Failed loading plugin "product-id": cannot import name 'which'
Last metadata expiration check: 0:00:17 ago on Tue 08 Aug 2023 07:58:51 AM UTC.
Package java-17-openjdk-1:17.0.8.0.7-2.el8.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
```

**3. start the jenkins service:**

```
[root@jenkins ~]# service jenkins start
Starting jenkins (via systemctl):                          [  OK  ]
[root@jenkins ~]# service jenkins status
● jenkins.service - Jenkins Continuous Integration Server
   Loaded: loaded (/usr/lib/systemd/system/jenkins.service; disabled; vendor preset: disabled)
   Active: active (running) since Tue 2023-08-08 08:13:08 UTC; 11s ago

jenkins page -> 

again come to terminal -> 
[root@jenkins ~]# cat /var/lib/jenkins/secrets/initialAdminPassword
932e259ed9c14a0195469c0a92907ae6


again go to jenkins and install the default plugins.

enter the details of user, password and email -> click to continue

Instance Configuration:
https://8080-port-228d0ad743964afd.labs.kodekloud.com/

Jenkins is ready!
```

**4. Click on `Finish` & `Confirm` to complete the task successfully**




