



## Questions:
Nautilus project developers are planning to start testing on a new project. As per their meeting with the DevOps team, they want to test containerized environment application features. As per details shared with DevOps team, we need to accomplish the following task:



a. Pull busybox:musl image on App Server 2 in Stratos DC and re-tag `create new tag` this image as busybox:news.


## Solution:  


**1. At first login on to app server &  Switch to root user**

```

thor@jump_host ~$ ssh steve@stapp02
steve@stapp02's password: Am3ric@

[steve@stapp02 ~]$ sudo su -
[sudo] password for steve: Am3ric@
```

**2. Run the Below command to check existing docker images**

```
[root@stapp02 ~]# docker images
```

**3. Run the Below command to  pull the docker image on the server**

```
[root@stapp02 ~]# docker pull busybox:musl
```

**4. Now retag ( create new tag ) as per the task**

```

[root@stapp02 ~]# docker image tag busybox:musl  busybox:news
[root@stapp02 ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
busybox      musl      7b10eaf5a34c   9 days ago   1.41MB
busybox      news      7b10eaf5a34c   9 days ago   1.41MB
```

**5.  Click on `Finish` & `Confirm` to complete the task successfully**

