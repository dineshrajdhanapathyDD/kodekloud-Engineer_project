

## Questions:
Last week the Nautilus DevOps team met with the application development team and decided to containerize several of their applications. The DevOps team wants to do some testing per the following:



Install docker-ce and docker-compose packages on App Server 3.

Start docker service.


## Solution:  

**1. Login on the App server given in task & switch to root user**

```
thor@jump_host ~$ ssh banner@stapp03

banner@stapp03's password:BigGr33n

[banner@stapp03 ~]$ sudo su -

[sudo] password for banner:BigGr33n
```

**2. Docker compose download form git from the given link below**

```
[root@stapp03 ~]# curl -L "https://github.com/docker/compose/releases/download/1.28.6/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0
100 11.6M  100 11.6M    0     0  8206k      0  0:00:01  0:00:01 --:--:--  270M
```

**3. Make sure to set  executable permission**

```
[root@stapp03 ~]# ll /usr/local/bin/docker-compose
-rw-r--r-- 1 root root 12212176 Jul 13 11:45 /usr/local/bin/docker-compose

[root@stapp03 ~]# chmod +x /usr/local/bin/docker-compose
[root@stapp03 ~]# ll /usr/local/bin/docker-compose
-rwxr-xr-x 1 root root 12212176 Jul 13 11:45 /usr/local/bin/docker-compose
```

**4. Validate by running version**

```
[root@stapp03 ~]# docker-compose --version
docker-compose version 1.28.6, build 5db8d86f
```

**5. To install docker-ce  need to configure repo from below link**

```
[root@stapp03 ~]# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

Loaded plugins: fastestmirror, ovl
adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
grabbing file https://download.docker.com/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo
```

**6. Install docker**

```
[root@stapp03 ~]# yum install docker-ce docker-ce-cli containerd.io
```

**7. Verify  docker installed successfully**

```
[root@stapp03 ~]# rpm -qa |grep docker
```

**8. Enable & start the docker service**

```
[root@stapp03 ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@stapp03 ~]# systemctl start  docker
[root@stapp03 ~]# systemctl status  docker
```

**9. Validate the task**

```
[root@stapp03 ~]# docker --version
Docker version 24.0.4, build 3713ee1
[root@stapp03 ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@stapp03 ~]# docker-compose --version
docker-compose version 1.28.6, build 5db8d86f
```

**10.  Click on `Finish` & `Confirm` to complete the task successful**