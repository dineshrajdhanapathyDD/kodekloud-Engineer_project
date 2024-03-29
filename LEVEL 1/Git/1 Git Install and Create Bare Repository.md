

## Questions:
The Nautilus development team shared requirements with the DevOps team regarding new application development.—specifically, they want to set up a Git repository for that project. Create a Git repository on `Storage server` in Stratos DC as per details given below:

1.Install `git` package using `yum` on `Storage server`

2.After that create a bare repository `/opt/news.git` {make sure to use exact name}.


## Solution:  

**1. At first login on storage server  & switch to root user**

```
thor@jump_host /$ ssh natasha@ststor01
natasha@ststor01's password: Bl@kW
[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
```

**2. Install git** 

```
[root@ststor01 ~]# yum install -y git

Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 0:01:01 ago on Thu Jul 27 18:32:02 2023.
Package git-2.39.3-1.el8_8.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
```

**3. Verify Installed git version & path where git need to initiated**

```
[root@ststor01 ~]# rpm -qa |grep git
git-core-doc-2.39.3-1.el8_8.noarch
crypto-policies-20221215-1.gitece0092.el8.noarch
git-core-2.39.3-1.el8_8.x86_64
libnsl2-1.2.0-2.20180605git4a062cf.el8.x86_64
git-2.39.3-1.el8_8.x86_64
crypto-policies-scripts-20221215-1.gitece0092.el8.noarch
```

**4. Make sure you init  bare  repo in given path**

```
[root@ststor01 ~]# cd /opt/
[root@ststor01 opt]# git init  --bare news.git
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
Initialized empty Git repository in /opt/news.git/

[root@ststor01 opt]# git status
fatal: not a git repository (or any of the parent directories): .git
```

**5. Click on `Finish` & `Confirm` to complete the task successful**






