

## Question :

There are new requirements to automate a backup process that was performed manually by the xFusionCorp Industries system admins team earlier. To automate this task, the team has developed a new bash script xfusioncorp.sh. They have already copied the script on all required servers, however they did not make it executable on one the app server i.e App Server 1 in Stratos Datacenter.

Please give executable permissions to `/tmp/xfusioncorp.sh` script on App Server 1. Also make sure every user can execute it.

## Solution:  

**1. Login on   App server as per the task**

```
thor@jump_host /$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n
```

**2. Give executable permissions to /tmp/xfusioncorp.sh script on App Server 1**

```
[root@stapp01 ~]# ls -ltr /tmp/xfusioncorp.sh
---------- 1 root root 40 May 02 13:04 /tmp/xfusioncorp.sh
[root@stapp01 ~]# 
[root@stapp01 ~]# chmod o+rx /tmp/xfusioncorp.sh
[root@stapp01 ~]# 
[root@stapp01 ~]# ls -ltr /tmp/xfusioncorp.sh
-------r-x 1 root root 40 May 02 13:04 /tmp/xfusioncorp.sh
[root@stapp01 ~]# 
[root@stapp01 ~]# exit
logout
[tony@stapp01 ~]$ 
[tony@stapp01 ~]$ /tmp/xfusioncorp.sh 
Welcome To KodeKloud 
```

**3.  Click on `Finish` & `Confirm` to complete the task successful**