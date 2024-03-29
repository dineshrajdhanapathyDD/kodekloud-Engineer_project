

## Questions:

The Nautilus application development team has been working on a project repository `/opt/apps.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`. They recently shared the following requirements with DevOps team:


One of the developers is working on `feature` branch and their work is still in progress, however there are some changes which have been pushed into the `master` branch, the developer now wants to `rebase` the `feature` branch with the `master` branch without loosing any data from the `feature` branch, also they don't want to add any `merge commit` by simply merging the `master` branch into the `feature` branch. Accomplish this task as per requirements mentioned.

Also remember to push your changes once done.


## Solution:  

**1. At first login on to the storage server  & switch to the root user**

```
thor@jump_host ~$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:4qpVu6YA9q0V+ODkZ6Pp9oE1dpU/8qYDAoPYAKSjL7E.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
natasha@ststor01's password: Bl@kW

[natasha@ststor01 kodekloudrepos]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for natasha: Bl@kW
```

**2. check repo and go to kodekloudrepos directory: cd /usr/src/kodekloudrepos**

```
[root@ststor01 ~]# cd /usr/src/kodekloudrepos
[root@ststor01 kodekloudrepos]# ls
apps

[root@ststor01 kodekloudrepos]# cd apps
[root@ststor01 apps]# ls
feature.txt  info.txt
```

**3. Rebase from master branch: git rebase master feature**

```
[root@ststor01 apps]# git rebase master feature
Successfully rebased and updated refs/heads/feature.
```

**4. Set git rebase: git config --global pull.rebase true**

```
[root@ststor01 apps]# git config --global pull.rebase true
```

**5. Pull from origin feature branch: git pull origin feature**

```
[root@ststor01 apps]# git pull origin feature
From /opt/apps
 * branch            feature    -> FETCH_HEAD
warning: skipped previously applied commit e3cf265
hint: use --reapply-cherry-picks to include skipped commits
hint: Disable this message with "git config advice.skippedCherryPicks false"
Successfully rebased and updated refs/heads/feature.
```

**6. Push changes: git push origin feature**

```
[root@ststor01 apps]# git push origin feature
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 36 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 315 bytes | 315.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/apps.git
   ef3ab8e..16dd050  feature -> feature
[root@ststor01 apps]# 


[root@ststor01 apps]# git status
On branch feature
nothing to commit, working tree clean
[root@ststor01 apps]# 
```

**7. Click on `Finish` & `Confirm` to complete the task successfully**
