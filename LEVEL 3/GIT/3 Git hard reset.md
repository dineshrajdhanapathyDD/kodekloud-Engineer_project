

## Questions:

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/news` present on `Storage server` in `Stratos DC`. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the (HEAD) and the branch itself to a commit with message `add data.txt file`. Find below more details:

In `/usr/src/kodekloudrepos/news` git repository, reset the git commit history so that there are only two commits in the commit history i.e `initial commit` and `add data.txt file`.

Also make sure to push your changes.


## Solution:  

**1. At first login on to the storage server  & switch to the root user**

```

thor@jump_host ~$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:X61ZgXW/oLGeP1i3OTjo4AosWTSgmayXJtmUNBpjkPU.
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

**2. Check repo on cd**   

```

[root@ststor01 ~]# cd /usr/src/kodekloudrepos/news
```

**3. check the log**

```

[root@ststor01 news]# git log
commit fe18b59cbed23e6aa69e8a5be0663cb9f1c293c1 (HEAD -> master, origin/master)
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:36 2023 +0000

    Test Commit10

commit a005ee5321b4ff439663b19aa8148ade25b1e55a
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:36 2023 +0000

    Test Commit9

commit 876cf91ae4ba459ac0f525427c3a425737d3c50f
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:36 2023 +0000

    Test Commit8

commit dae25faa651ac35676ee6ca44617f2f160886cb3
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:35 2023 +0000

    Test Commit7

commit f8a498c0cdb31379daee8b62a873c7fdebf2babc
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:35 2023 +0000

    Test Commit6

commit b30391ae7635ef96f06c8dcdb567693fee3d7cf2
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:35 2023 +0000

    Test Commit5

commit ed36370a2e11bea480d8c0e7eaac417fc8dadafa
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:35 2023 +0000

    Test Commit4

commit f8f376fbec51917ab22023c8311f77ca39e77658
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:34 2023 +0000

    Test Commit3

commit b442b26535fa02a0ff532c7f561136a2b9dbe515
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:34 2023 +0000

    Test Commit2

commit 622d5c18b9606efcfcad96a16fbf91ad574fd520
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:34 2023 +0000

    Test Commit1

commit dea57193a3ca26e0b600bdab0e8dd9b59d116525
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:34 2023 +0000

    add data.txt file

commit 2da6626d859e65aa989035d02463637c954dafbd
Author: Admin <admin@kodekloud.com>
Date:   Fri Sep 22 20:52:34 2023 +0000

    initial commit

[1]+  Stopped                 git log
```


**4. Create the reset hard for data.txt file**

```

[root@ststor01 news]# git reset --hard d9bdf915163027619d1835883858452af8a41f7b
HEAD is now at d9bdf91 add data.txt file


[root@ststor01 news]# git log
commit d9bdf915163027619d1835883858452af8a41f7b (HEAD -> master)
Author: Admin <admin@kodekloud.com>
Date:   Sat Sep 23 18:36:42 2023 +0000

    add data.txt file

commit bbc16df75100b63be190db44e46058246c14cf96
Author: Admin <admin@kodekloud.com>
Date:   Sat Sep 23 18:36:42 2023 +0000

    initial commit
```

**5. check git branch**

```

[root@ststor01 news]# git branch
* master
```

**6. check the git configuration**

```

[root@ststor01 news]# git config --list
user.email=admin@kodekloud.com
user.name=Admin
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=/opt/news.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master
```

**7. create the git push force**

```
 
[root@ststor01 news]# git push -f
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/news.git
 + 892c445...d9bdf91 master -> master (forced update)
```

**8. Git push to the branch as well master**

```

[root@ststor01 news]# git push -u origin master 
Everything has upto date
```

**9. check the git status**

```

[root@ststor01 news]# git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

```

**10. Click on `Finish` & `Confirm` to complete the task successfully**

