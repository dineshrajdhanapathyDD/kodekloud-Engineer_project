


## Questions:
There was some users data copied on Nautilus App Server 1 at `/home/usersdata` location by the Nautilus production support team in Stratos DC. Later they found that they mistakenly mixed up different user data there. Now they want to filter out some user data and copy it to another location. Find the details below:



On App Server 1 find all files (not directories) owned by user yousuf inside `/home/usersdata` directory and copy them all while keeping the folder structure (preserve the directories path) to `/official` directory.

## solutions:

**1. At first login on App server   &  Switch to  root user**

```
thor@jump_host ~$ ssh tony@stapp01
password : Ir0nM@n

[tony@stapp01 ~]$ sudo su -

password : Ir0nM@n
```

**2. List the files and folders in the given folder path in task**

```
[root@stapp01 ~]# ll /home/usersdata/

[root@stapp01 ~]# ll /official/
0
find /home/usersdata/ -type f -user yousuf |wc -l
1383
```

**3. As per the task find the files name by user & copy with folder structure**


- find /home/usersdata/ -type f -user yousuf -exec cp --parents {} /official \;

**4. List the files copied in the destination folder given in task**

```
[root@stapp01 ~]# ll /offical/home/usersdata/

total 180
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**