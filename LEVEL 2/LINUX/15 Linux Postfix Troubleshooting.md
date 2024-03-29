

## Questions:

Some users of the monitoring app have reported issues with xFusionCorp Industries mail server. They have a mail server in Stork DC where they are using postfix mail transfer agent. Postfix service seems to fail. Try to identify the root cause and fix it.


## Solution: 

**1. Login on   Mail  server as per the task and switch root user**

```
thor@jump_host /$ ssh groot@stmail01
groot@stmail01's password: Gr00T123

[groot@stmail01 ~]$ sudo su -
```

**2. Start postfix service , check status with -l for details error output** 

```
[root@stmail01 ~]# systemctl start postfix

[root@stmail01 ~]# systemctl status postfix -l
```

**3. As we identify the issue in configuration file edit and do the changes**

```
[root@stmail01 ~]# cat /etc/postfix/main.cf |grep inet_interface

#inet_interfaces = $myhostname

#inet_interfaces = $myhostname, localhost

inet_interfaces = localhost  - not in #

[root@stmail01 ~]# vi /etc/postfix/main.cf

we have to find the local host interfaces in another terminal
 
we use /inet- shown receiveing mail 
Add # for inet_interfaces = localhost => how to enter # use -i (insert)
#inet_interfaces = localhost - add and change it then enter 
:wq exit from that terminal


[root@stmail01 ~]# cat /etc/postfix/main.cf |grep inet_interface
inet_interfaces = all

#inet_interfaces = $myhostname

#inet_interfaces = $myhostname, localhost

#inet_interfaces = localhost   - this line difference from above line
```

**4. Post configuration saved start the services and check the status**

```
[root@stmail01 ~]# systemctl start postfix


[root@stmail01 ~]# systemctl status postfix
```

**5. validate the task by telnet the port 25** 

```
thor@jump_host /$ telnet stmail01 25


220 stmail01.stratos.xfusioncorp.com ESMTP Postfix 
```

**6. Click on `Finish` & `Confirm` to complete the task successful**
