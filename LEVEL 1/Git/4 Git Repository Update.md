

## Questions:
The Nautilus development team started with new project development. They have created different Git repositories to manage respective project's source code. One of the repositories `/opt/cluster.git` was created recently. The team has given us a sample `index.html` file that is currently present on `jump host` under `/tmp` directory. The repository has been cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`.

Copy sample `index.html` file from `jump host` to `storage server` under cloned repository at `/usr/src/kodekloudrepos/cluster`, further `add/commit` the file and push to the `master` branch.


## Solution:   

**1.  At 1st SCP index.html file from jump server to the storage server**    

```
thor@jump_host ~$  sudo scp /tmp/index.html  natasha@ststor01:/tmp

[sudo] password for thor: mjolnir123
natasha@ststor01's password:  Bl@kW
index.html                                                             100%   27    83.1KB/s   00:00 
```

**2. Login on storage server  & Switch Root user**

```
thor@jump_host ~$ ssh natasha@ststor01
natasha@ststor01's password: Bl@kW

[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
```

**3. Go to project repositories as per the task**

```
[root@ststor01 ~]# cd   /usr/src/kodekloudrepos/cluster/

[root@ststor01 cluster]# ls
info.txt  welcome.txt
```

**4. Copy html file from tmp to repo**

```
[root@ststor01 cluster]# cp /tmp/index.html  .
[root@ststor01 cluster]# ls
index.html  info.txt  welcome.txt
```

**5.  Add and commit the index.html file**

```
[root@ststor01 cluster]# git add index.html
[root@ststor01 cluster]# git commit -m "add index.html"
[master 7721f79] add index.html
 1 file changed, 1 insertion(+)
 create mode 100644 index.html
```

**6. Finally push master branch to the origin.**

```
[root@ststor01 cluster]# git push -u origin master
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 36 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 330 bytes | 330.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/cluster.git
   de80569..7721f79  master -> master
branch 'master' set up to track 'origin/master'.
```

**7. Click on `Finish` & `Confirm` to complete the task successful**



