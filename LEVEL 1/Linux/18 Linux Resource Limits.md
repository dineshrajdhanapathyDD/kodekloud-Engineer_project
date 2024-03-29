

## Questions:
On our `Storage` server in `Stratos Datacenter` we are having some issues where `nfsuser` user is holding hundred of processes, which is degrading the performance of the server. Therefore, we have a requirement to limit its maximum processes. Please set its maximum process limits as below:

- a. soft limit = `1025`

- b. hard_limit = `2027`

## Solution:  

**1. At first login to one storage server  &  Switch to the root user**

```
thor@jump_host /$ ssh natasha@ststor01
natasha@ststor01's password:  Bl@kW

[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha:  Bl@kW
```

**2.  Edit limit.conf file as below vi /etc/security/limits.conf**

vi /etc/security/limits.conf

```
open new terminal -paste below code:
nfsuser         soft    nproc   1025
nfsuser         hard    nproc   2027
:wq

[root@ststor01 ~]# cat /etc/security/limits.conf | grep nproc | grep -v ^#
nfsuser         soft    nproc   1025
nfsuser         hard    nproc   2027
```

**3. Click on `Finish` & `Confirm` to complete the task successful**

