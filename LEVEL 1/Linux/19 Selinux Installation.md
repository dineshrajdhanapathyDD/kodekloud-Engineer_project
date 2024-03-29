

## Question:

The xFusionCorp Industries security team recently did a security audit of their infrastructure and came up with ideas to improve the application and server security. They decided to use SElinux for an additional security layer. They are still planning how they will implement it; however, they have decided to start testing with app servers, so based on the recommendations they have the following requirements:



 - Install the required packages of SElinux on App server 2 in Stratos Datacenter and disable it permanently for now; it will be enabled after making some required configuration changes on this host. Don't worry about rebooting the server as there is already a reboot scheduled for tonight's maintenance window. Also ignore the status of SElinux command line right now; the final status after reboot should be disabled.


## Solutions :

**1. At first login on storage server & Switch to  root user**

```
thor@jump_host ~$ ssh steve@stapp02

steve@stapp02's password: Am3ric@

[steve@stapp02 ~]$ sudo su -

[sudo] password for steve: Am3ric@
```

**2. Install the SElinux**

```
[root@stapp02 ~]# yum -y install selinux*

Complete!
```

**3. Check the existing  SElinux  status**

```
[root@stapp02 ~]# sestatus

SELinux status:                 disabled

[root@stapp02 ~]# cat /etc/selinux/config | grep SELINUX

# SELINUX= can take one of these three values:
SELINUX=enforcing
# SELINUXTYPE= can take one of three values:
SELINUXTYPE=targeted
```

**4. Edit the /etc/selinux/config  file and correct the changes as per below**

```
[root@stapp02 ~]# vi /etc/selinux/config
 new terminal open

SELINUX=enforcing change it "disabled" using -i for insert

SELINUX=disabled  

then cltr+c  type :wq(exit terminal)

[root@stapp02 ~]# cat /etc/selinux/config | grep SELINUX

# SELINUX= can take one of these three values:
SELINUX=disabled
# SELINUXTYPE= can take one of three values:
SELINUXTYPE=targeted 
```

**5.  Validate the task by sestatus**

```
[root@stapp02 ~]# sestatus

SELinux status:                 disabled
```

**6. Click on `Finish` & `Confirm` to complete the task successful.**


