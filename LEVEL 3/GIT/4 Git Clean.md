

## Questions:

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/ecommerce` present on `Storage server` in `Stratos DC`. One of the developers mistakenly created a couple of files under this repository, but now they want to clean this repository without adding/pushing any new files. Find below more details:


Clean the `/usr/src/kodekloudrepos/ecommerce` git repository without adding/pushing any new files, make sure `git status` is clean.



## Solution:

**1. At first login on to the storage server  & switch to the root user**

```

thor@jump_host ~$ ssh natasha@ststor01
natasha@ststor01's password: Bl@kW
Last login: Sun Sep 24 20:53:20 2023 from 172.16.238.3
[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
Last login: Sun Sep 24 20:53:32 UTC 2023 on pts/0
[root@ststor01 ~]# 
```

**2. Check repo on cd**

```
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/ecommerce
```

**3. check the clean**

```

[root@ststor01 ecommerce]# git stash pop 1
error: refs/stash@{1} is not a valid reference
[root@ststor01 ecommerce]# git stash
Saved working directory and index state WIP on master: fb03f08 add data.txt file
```

**4. Add all files**

```
[root@ststor01 ecommerce]# git add .
```

**5. Commit the files with message**

```

[root@ststor01 ecommerce]# git commit -m 'commit'
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

**6. Git push to the branch as well master** 

```

[root@ststor01 ecommerce]# git push origin master
Everything up-to-date
```

**7.Click on `Finish` & `Confirm` to complete the task successfully**