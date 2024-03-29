

## Question:

The system admins team of xFusionCorp Industries has set up a new tool on all app servers, as they have a requirement to create a service user account that will be used by that tool. They are finished with all apps except for App Server 1 in Stratos Datacenter.

Create a user named javed in App Server 1 without a home directory

## Solution:

**1. At first login on app server given in task & Switch to  root user** 

```
thor@jump_host /$ ssh tony@stapp01

tony@stapp01's password:Ir0nM@n

[tony@stapp01 ~]$ sudo su -

[sudo] password for tony:Ir0nM@n
```


**2. Check user javed is existed , if not then proceed with creation**

```
[root@stapp01 ~]# id javed

id: mohammed: no such user

[root@stapp01 ~]# cat /etc/passwd |grep javed
```


**3. create a user javed with below commands ( check user name in your task)**     

```
[root@stapp01 ~]# useradd -M javed
```


**4.  Validate the task by listing the file exist in  Home directory**   

```
[root@stapp01 ~]# id javed

uid=1002(javed) gid=1002(javed) groups=1002(javed)

[root@stapp01 ~]#

[root@stapp01 ~]# cat /etc/passwd |grep javed

javed:x:1002:1002::/home/javed:/bin/bash

[root@stapp01 ~]#

[root@stapp01 ~]# ll /home

total 8

drwx------ 1 ansible ansible 4096 Oct 15  2019 ansible

drwx------ 1 tony    tony    4096 Jan 25  2020 tony
```


**5. Click on `Finish` & `Confirm` to complete the task successful.**


