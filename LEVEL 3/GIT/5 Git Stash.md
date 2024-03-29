

## Questions:

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/official` present on `Storage server` in `Stratos DC`. One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:



Look for the stashed changes under `/usr/src/kodekloudrepos/official` git repository, and restore the stash with `stash@{1}` identifier. Further, commit and push your changes to the origin.


## Solution:  

**1. At first login on to the storage server  & switch to the root user**

```

thor@jump_host ~$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:624Sts025xVkhIVZs1pPofeeprbadu4CT1Z1yn9dz0c.
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
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/official
```

**3.  Restore the stash with `stash@{1}` identifier**

```

[root@ststor01 official]# git stash apply stash@{1}
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   welcome.txt
```

**4. Commit the files with message**

```

[root@ststor01 official]# git commit -m "commit"
[master f4e9308] commit
 1 file changed, 1 insertion(+)
 create mode 100644 welcome.txt
```

**5. Git push to the branch as well master** 

```

[root@ststor01 official]# git push -u origin master
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 36 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 295 bytes | 295.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/official.git
   fa19c28..f4e9308  master -> master
branch 'master' set up to track 'origin/master'.
```

**6. Click on `Finish` & `Confirm` to complete the task successfully**
