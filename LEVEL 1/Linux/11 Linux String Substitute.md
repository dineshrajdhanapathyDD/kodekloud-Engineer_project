

## Questions:
The backup server in the Stratos DC contains several template XML files used by the Nautilus application. However, these template XML files must be populated with valid data before they can be used. One of the daily tasks of a system admin working in the xFusionCorp industries is to apply string and file manipulation commands!



Replace all occurances of the string About to Software on the XML file `/root/nautilus.xml` located in the backup server.


## Solution: 

**1. At first login on Backup server  & Switch to  root user**

```
thor@jump_host ~$ ssh clint@stbkp01
clint@stbkp01's password:H@wk3y3

[clint@stbkp01 ~]$ sudo su -
[sudo] password for clint:  H@wk3y3
```

**2. Pull out the string About on the XML file /root/nautilus.xml**

```
[root@stbkp01 ~]# cat /root/nautilus.xml  |grep About  | wc -l

[root@stbkp01 ~]# cat /root/nautilus.xml  |grep About
```

**4. Replace the string  About  to Cloud with the help of sed command**

```
[root@stbkp01 ~]# sed -i 's/About/Software/g' /root/nautilus.xml
```

**5. confirm the change in string Cloud**

```
[root@stbkp01 ~]# cat /root/nautilus.xml  |grep Software

[root@stbkp01 ~]# cat /root/nautilus.xml  |grep Software |wc -l
66
[root@stbkp01 ~]# cat /root/nautilus.xml  |grep About  | wc -l
0
```

**6. Click on `Finish` & `Confirm` to complete the task successful.**