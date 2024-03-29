

## Questions:
The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:



- a. Install tomcat server on App Server 2 using yum.

- b. Configure it to run on port 5004.

- c. There is a ROOT.war file on Jump host at location /tmp. Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e without specifying any sub-directory anything like this http://URL/ROOT .

## Solution:  

**1. At first Copy the war file from the jump host to the app server given in the task**  

```

thor@jump_host ~$ scp /tmp/ROOT.war steve@stapp02:/tmp
steve@stapp02's password: Am3ric@
ROOT.war                                             100% 4529     7.0MB/s   00:00
```

**2. Login on app server as per the task & Switch to  root user**

```

thor@jump_host ~$ ssh steve@stapp02
steve@stapp02's password: Am3ric@
[steve@stapp02 ~]$ sudo su -

[sudo] password for steve: Am3ric@
```

**3. Install  Tomcat  Packages  & configure   ahead**


```
[root@stapp02 ~]# yum install -y tomcat
```

**4. Edit the config file of the tomcat server and replace the port mentioned in the given task Connector port of non-ssl**

```

[root@stapp02 ~]# vi /usr/share/tomcat/conf/server.xml

open another terminal /port chnage the connector port ="8080"  to use insert option to edit 
==>Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
to==>Connector port="5004" protocol="HTTP/1.1"
               connectionTimeout="20000"
next ==>Connector executor="tomcatThreadPool"
               port="8080" protocol="HTTP/1.1"
to ==>Connector executor="tomcatThreadPool"
               port="5004" protocol="HTTP/1.1"
:wq to exit
[root@stapp02 ~]# cat /usr/share/tomcat/conf/server.xml |grep port
```

**5. copy war file from tmp to tomcat root directory**

```

[root@stapp02 ~]# cp /tmp/ROOT.war /usr/share/tomcat/webapps

[root@stapp02 ~]# ll /usr/share/tomcat/webapps/
```

**6. Start the tomcat service & check the status.**  

```

[root@stapp02 ~]# systemctl start tomcat

[root@stapp02 ~]# systemctl status tomcat
```

**7. Validate the task by the below commands**

```
[root@stapp02 ~]# curl -i http://stapp02:5004
```

**8. Click on `Finish` & `Confirm` to complete the task successfully**

