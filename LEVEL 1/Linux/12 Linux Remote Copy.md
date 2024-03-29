


## Question :
One of the Nautilus developers has copied confidential data on the jump host in Stratos DC. That data must be copied to one of the app servers. Because developers do not have access to app servers, they asked the system admins team to accomplish the task for them.

Copy `/tmp/nautilus.txt.gpg` file from jump server to `App Server 1` at location `/home/web.`


## Solution:  

**1. At first scp the file to stapp03 ( Please refer server as per given in your task)**

```
thor@jump_host ~$ ll /tmp/nautilus.txt.gpg

thor@jump_host ~$ sudo scp /tmp/nautilus.txt.gpg  tony@stapp01:/tmp/

[sudo] password for thor: mjolnir123

tony@stapp01's password:  Ir0nM@n

nautilus.txt.gpg - shown like this
```

**2. Login on app server & switch to root user**

```
thor@jump_host ~$ ssh tony@stapp01

password :  Ir0nM@n

[tony@stapp01 ~]$ sudo su -

password :  Ir0nM@n
```

**3. post copied, move file from /tmp location  to correct location ( /home/web/)**

```
[tony@stapp01 ~]$ mv /tmp/nautilus.txt.gpg  /home/web/

[tony@stapp01 ~]$ ll /home/web/
```

**4.  Click on `Finish` & `Confirm` to complete the task successful**

