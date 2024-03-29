

 


## Question :  
The system admin team of xFusionCorp Industries has noticed an issue with some servers in Stratos Datacenter where some of the servers are not in sync w.r.t time. Because of this, several application functionalities have been impacted. To fix this issue the team has started using common/standard NTP servers. They are finished with most of the servers except App Server 1. Therefore, perform the following tasks on this server:

- Install and configure NTP server on App Server 1.

- Add NTP server 1.sg.pool.ntp.org in NTP configuration on App Server 1.

- Please do not try to start/restart/stop ntp service, as we already have a restart for this service scheduled for tonight and we don't want these changes to be applied right now.


## solution:

**1. At first login on App server   &  Switch to  root user**

```
thor@jump_host ~$ ssh tony@stapp01
password : Ir0nM@n

[steve@stapp02 ~]$ sudo su -
password : Ir0nM@n
```

**2. Check NTP is installed , if not then install on the server**

```
[root@stapp01 ~]# rpm -qa |grep ntp

[root@stapp01 ~]# yum install -y ntp
```

**3. Confirm package install successfully**

```
[root@stapp01 ~]# rpm -qa |grep ntp
```

**4. Add NTP server in configuration file**

```
[root@stapp01 ~]# vi /etc/ntp.conf 

[root@stapp01 ~]# cat /etc/ntp.conf |grep sg.pool
```

**5. Check NTP daemon status**  

```
[root@stapp01 ~]# ntpstat

[root@stapp01 ~]# systemctl status ntpd
```

**6. Start NTP daemon enable and check the status**

```
[root@stapp01 ~]# systemctl enable ntpd

[root@stapp01 ~]# systemctl start  ntpd

[root@stapp01 ~]# systemctl status ntpd
```

**7. Validate the task by NTP status**

```
[root@stapp01 ~]# ntpstat
```




