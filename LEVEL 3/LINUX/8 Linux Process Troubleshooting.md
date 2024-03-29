

## Questions:

The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.



Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not hosted any code yet on these servers so you need not to worry about if Apache isn't serving any pages or not, just make sure service is up and running. Also, make sure Apache is running on port 3002 on all app servers.


## Solution: 

**1. At first identify the app server which has issue by running the below commands**

```

thor@jump_host ~$ telnet stapp01 3002

thor@jump_host ~$ telnet stapp02 3002

thor@jump_host ~$ telnet stapp03 3002
```

**2. login on app server which has connection refuse error & switch to root**  

```

thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n
[tony@stapp01 ~]$ sudo su -

[sudo] password for tony:  Ir0nM@n
```

**3. Run the Below command to check the existing Apache HTTPd service**    

```

[root@stapp01 ~]# systemctl start httpd
Job for httpd.service failed because the control process exited with error code. See "systemctl status httpd.service" and "journalctl -xe" for details.

[root@stapp01 ~]# systemctl status httpd
```

**4. With the help of  netstat check which service is using your port and whats is PID. In below snipping ( refer to  below video for more details)**

```

[root@stapp01 ~]# netstat -tulnp

tcp        0      0 127.0.0.1:8087          0.0.0.0:*               LISTEN      615/sendmail: accep
```

**5. Confirm the PID as per  the netstat output**

```

[root@stapp01 ~]# ps -ef  |grep sendmail


[root@stapp01 ~]# kill 610

[root@stapp01 ~]# ps -ef  |grep sendmail
```

**6.  Start the httpd service & check the status.**

```

[root@stapp01 ~]# systemctl start httpd

[root@stapp01 ~]# systemctl status  httpd
```

**7.  Validate Apache HTTPd running  as per the task request**
 
- open the new terminal:
`thor@jump_host ~$ telnet stapp01 3002`


**8.  Click on `Finish` & `Confirm` to complete the task successfully**