

## Questions:

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/beta` present on `Storage server` in `Stratos DC`. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:

- In `/usr/src/kodekloudrepos/beta` git repository, revert the latest commit ` HEAD ` to the previous commit (JFYI the previous commit hash should be with `initial commit)` message ).

- Use `revert beta` message (please use all small letters for commit message) for the new revert commit.



## Solution:  

**1. At first login on storage server  & switch to root user**

```

thor@jump_host ~$ ssh natasha@ststor01
natasha@ststor01's password: Bl@kW

[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
```

**2. Check repo  git status**

```

[root@ststor01 ~]# ls /usr/src/kodekloudrepos/beta/
beta.txt
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/beta/
[root@ststor01 beta]# git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        beta.txt

nothing added to commit but untracked files present (use "git add" to track)
```

**3. Check Git logs**

```

[root@ststor01 beta]# git log
commit f266712647b8dcaa67918e6d560c3dba8b04a315 (HEAD -> master, origin/master)
Author: Admin <admin@kodekloud.com>
Date:   Sat Sep 2 08:45:35 2023 +0000

    add data.txt file

commit 98b039ab4aabfecbdb094850bb3e94f6b3b12f15
Author: Admin <admin@kodekloud.com>
Date:   Sat Sep 2 08:45:35 2023 +0000

    initial commit
```

**4. Run command revert HEAD and Add , Commit with given message in task**

```

[root@ststor01 beta]# git revert HEAD
[master 59571b9] Revert "add data.txt file"
 1 file changed, 1 insertion(+)
 create mode 100644 info.txt

{
optional:-

Revert "add data.txt file"

This reverts commit f266712647b8dcaa67918e6d560c3dba8b04a315.

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Your branch is up to date with 'origin/master'.
#
# Changes to be committed:
#       new file:   info.txt
#
# Untracked files:
#       beta.txt
}


[root@ststor01 beta]# git add .
[root@ststor01 beta]# git commit -m " revert beta"
[master c3b5a1b]  revert beta
 1 file changed, 1 insertion(+)
 create mode 100644 beta.txt
```

**5. Validate the task**

```

[root@ststor01 beta]# git log
commit c3b5a1bdec1bae906bacd0d2a49b914fecb0255d (HEAD -> master)
Author: Admin <admin@kodekloud.com>
Date:   Sat Sep 2 08:54:28 2023 +0000

     revert beta

commit 59571b9fc7a954f53a5fd5d2a79e277d8a043c9a
Author: Admin <admin@kodekloud.com>
Date:   Sat Sep 2 08:51:33 2023 +0000

    Revert "add data.txt file"
    
    This reverts commit f266712647b8dcaa67918e6d560c3dba8b04a315.

commit f266712647b8dcaa67918e6d560c3dba8b04a315 (origin/master)
Author: Admin <admin@kodekloud.com>
Date:   Sat Sep 2 08:45:35 2023 +0000

    add data.txt file

commit 98b039ab4aabfecbdb094850bb3e94f6b3b12f15
Author: Admin <admin@kodekloud.com>
Date:   Sat Sep 2 08:45:35 2023 +0000

    initial commit
```

**6. Click on `Finish` & `Confirm` to complete the task successful**
