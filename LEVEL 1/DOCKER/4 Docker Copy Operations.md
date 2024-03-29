

## Questions:
The Nautilus DevOps team has some conditional data present on `App Server 3` in `Stratos Datacenter`. There is a container `ubuntu_latest` running on the same server. We received a request to copy some of the data from the docker host to the container. Below are more details about the task:

On `App Server 3` in `Stratos Datacenter` copy an encrypted file `/tmp/nautilus.txt.gpg` from docker host to `ubuntu_latest` container `running on same server` in `/usr/src/` location. Please do not try to modify this file in any way.


## Solution:  

**1. At first login on app server  as per the task & Switch to  root user**

```
thor@jump_host ~$ ssh banner@stapp03
banner@stapp03's password: BigGr33n

[banner@stapp03 ~]$ sudo su -
[sudo] password for banner: BigGr33n
```

**2. Run Below command to check existing docker container running**

```
[root@stapp03 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
f6064f1dde44        ubuntu              "/bin/bash"         4 minutes ago       Up 4 minutes                            ubuntu_latest
```

**3. Copy an encrypted file /tmp/nautilus.txt.gpg on docker container ubuntu_latest**

```
[root@stapp03 ~]# docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/usr/src
```

**4. Validate the task by running the command**

```
[root@stapp03 ~]# docker exec ubuntu_latest  ls -ltr /usr/src
total 4
-rw-r--r-- 1 root root 105 Aug  2 13:04 nautilus.txt.gpg
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**
