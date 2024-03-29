

## Questions:

The xFusionCorp development team added updates to the project that is maintained under `/opt/beta.git` repo and cloned under `/usr/src/kodekloudrepos/beta`. Recently some changes were made on Git server that is hosted on `Storage server` in `Stratos DC`. The DevOps team added some new Git remotes, so we need to update remote on `/usr/src/kodekloudrepos/beta` repository as per details mentioned below:

a. In `/usr/src/kodekloudrepos/beta` repo add a new remote `dev_beta` and point it to `/opt/xfusioncorp_beta.git` repository.

b. There is a file `/tmp/index.html` on same server; copy this file to the repo and add/commit to master branch.

c. Finally push `master` branch to this new remote origin.


## Solution:   

**1. At first login on storage server  & Switch to  root user**

```

thor@jump_host ~$ ssh natasha@ststor01
natasha@ststor01's password: Bl@kW

[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
```

**2. Navigate to the cloned directory**

```

[root@ststor01 ~]# cd /usr/src/kodekloudrepos/beta
[root@ststor01 beta]# ls
info.txt
```

**3. Add Remote repo as per the task**

```
[root@ststor01 beta]# git remote add dev_beta /opt/xfusioncorp_beta.git
```

**4. Copy HTML file from tmp to repo and add**

```

[root@ststor01 beta]# cp /tmp/index.html .
[root@ststor01 beta]# ls
index.html  info.txt
```

**5. Git initialize the new remote repo** 

```

[root@ststor01 beta]# git init
Reinitialized existing Git repository in /usr/src/kodekloudrepos/beta/.git/
```

**6.  Add and commit the index.html file**

```

[root@ststor01 beta]# git add index.html
[root@ststor01 beta]# git commit -m "add index.html"
[master 57bffa6] add index.html
 1 file changed, 10 insertions(+)
 create mode 100644 index.html
```

**7. Finally push the master branch to this new remote origin.**

```

[root@ststor01 beta]# git push -u dev_beta  master
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 36 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 583 bytes | 583.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/xfusioncorp_beta.git
 * [new branch]      master -> master
branch 'master' set up to track 'dev_beta/master'.
```

**8. Click on `Finish` & `Confirm` to complete the task successfully.**