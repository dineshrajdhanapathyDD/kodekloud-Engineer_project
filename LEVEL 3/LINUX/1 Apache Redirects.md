

## Questions:

The Nautilus devops team got some requirements related to some Apache config changes. They need to setup some redirects for some URLs. There might be some more changes need to be done. Below you can find more details regarding that:



httpd is already installed on app server 1. Configure Apache to listen on port 5004.

Configure Apache to add some redirects as mentioned below:

- a.) Redirect http://stapp01.stratos.xfusioncorp.com:<Port>/ to http://www.stapp01.stratos.xfusioncorp.com:<Port>/ i.e non www to www. This must be a permanent redirect i.e 301

- b.) Redirect http://www.stapp01.stratos.xfusioncorp.com:<Port>/blog/ to http://www.stapp01.stratos.xfusioncorp.com:<Port>/news/. This must be a temporary redirect i.e 302.

## Solution:  

**1. Login on   App server as per the task & switch to root user**

```

thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -

[sudo] password for tony: Ir0nM@n
```

**2. Confirm the current httpd version & configuration**

```

[root@stapp01 ~]# rpm -qa |grep httpd
httpd-tools-2.4.6-90.el7.centos.x86_64
httpd-2.4.6-90.el7.centos.x86_64
[root@stapp01 ~]# cat /etc/httpd/conf/httpd.conf |grep Listen
# Listen: Allows you to bind Apache to specific IP addresses and/or
# Change this to Listen on specific IP addresses as shown below to 
#Listen 12.34.56.78:80
Listen 8080
[root@stapp01 ~]# cat /etc/httpd/conf/httpd.conf |grep redirect
# 1) plain text 2) local redirects 3) external redirects
```

**3. Edit the configuration & change the port no as per the task**

```

[root@stapp01 ~]# vi /etc/httpd/conf/httpd.conf
open new terminal:
Listen 12.34.56.78:80
Listen 8080
edit like this 
#Listen 12.34.56.78:80
Listen 5004

control+c exit :wq

[root@stapp01 ~]# cat /etc/httpd/conf/httpd.conf |grep Listen
# Listen: Allows you to bind Apache to specific IP addresses and/or
# Change this to Listen on specific IP addresses as shown below to 
#Listen 12.34.56.78:80
Listen 5004
```

**4. As per the task we need to redirect permanent & temporary for which will create  main.conf**

```

[root@stapp01 ~]# ll /etc/httpd/conf.d/
total 20
-rw-r--r-- 1 root root 2926 Aug  8  2019 autoindex.conf
-rw-r--r-- 1 root root 1252 Dec 17  2019 php.conf
-rw-r--r-- 1 root root  366 Aug  8  2019 README
-rw-r--r-- 1 root root 1252 Aug  6  2019 userdir.conf
-rw-r--r-- 1 root root  824 Aug  6  2019 welcome.conf

[root@stapp01 ~]# vi /etc/httpd/conf.d/main.conf
open new terminal:
<VirtualHost *:5004>
ServerName stapp01.stratos.xfusioncorp.com
Redirect 301 / http://www.stapp01.stratos.xfusioncorp.com:5004/
</VirtualHost>
<VirtualHost *:5004>
ServerName www.stapp01.stratos.xfusioncorp.com:5004/blog/
Redirect 302 /blog/ http://www.stapp01.stratos.xfusioncorp.com:5004/news/
</VirtualHost>

control+c exit:wq

[root@stapp01 ~]# cat  /etc/httpd/conf.d/main.conf
<VirtualHost *:5004>
ServerName stapp01.stratos.xfusioncorp.com
Redirect 301 / http://www.stapp01.stratos.xfusioncorp.com:5004/
</VirtualHost>
<VirtualHost *:5004>
ServerName www.stapp01.stratos.xfusioncorp.com:5004/blog/
Redirect 302 /blog/ http://www.stapp01.stratos.xfusioncorp.com:5004/news/
</VirtualHost>
```

**5. Post file saved restart and check the httpd status**

```

[root@stapp01 ~]# systemctl restart httpd
[root@stapp01 ~]# systemctl status  httpd
Active: active (running)
```

**6. Validate the task by Curl**

```

[root@stapp01 ~]# curl http://stapp01.stratos.xfusioncorp.com:5004/
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://www.stapp01.stratos.xfusioncorp.com:5004/">here</a>.</p>
</body></html>
```
```
[root@stapp01 ~]# curl http://www.stapp01.stratos.xfusioncorp.com:5004
Welcome to the Nautilus Group!
```

```
[root@stapp01 ~]# curl http://www.stapp01.stratos.xfusioncorp.com:5004/blog/
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>302 Found</title>
</head><body>
<h1>Found</h1>
<p>The document has moved <a href="http://www.stapp01.stratos.xfusioncorp.com:5004/news/">here</a>.</p>
</body></html>
```

```
[root@stapp01 ~]# curl http://www.stapp01.stratos.xfusioncorp.com:5004/news/
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL /news/ was not found on this server.</p>
</body></html>
```

**8. Click on `Finish` & `Confirm` to complete the task successful**