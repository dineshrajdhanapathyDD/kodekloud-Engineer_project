

## Questions:

`Nautilus` system admin's team is planning to deploy a front end application for their backup utility on `Nautilus Backup Server`, so that they can manage the backups of different websites from a graphical user interface. They have shared requirements to set up the same; please accomplish the tasks as per detail given below:



- a. Install `Apache` Server on `Nautilus Backup Server` and configure it to use `3001` port (do not bind it to 127.0.0.1 only, keep it default i.e let Apache listen on server's IP, hostname, localhost, `127.0.0.1` etc).


b. Install `Nginx` webserver on `Nautilus Backup Server` and configure it to use `8093`.


c. Configure `Nginx` as a reverse proxy server for `Apache`.


d. There is a sample index file `/home/thor/index.html` on `Jump Host`, copy that file to `Apache's` document root.


e. Make sure to start `Apache` and `Nginx` services.


f. You can test final changes using `curl` command, e.g `curl http://<backup server IP or Hostname>:8093`.


## Solution:  

**1. At first copy sample index file from jump server to backup server**

```

thor@jump_host ~$ scp /home/thor/index.html clint@stbkp01:/tmp/
clint@stbkp01's password: H@wk3y3
```


**2. Login on Backup server  ssh clint@stbkp01**

```

thor@jump_host ~$ ssh clint@stbkp01
clint@stbkp01's password: H@wk3y3
```

**3. Switch to  root user : sudo su -**

```
[clint@stbkp01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for clint: H@wk3y3
[root@stbkp01 ~]# 
```

**4. Install the packages Apache & Nginx.**

```

[root@stbkp01 ~]# rpm -qa |grep httpd
[root@stbkp01 ~]# rpm -qa |grep nginx
[root@stbkp01 ~]# yum install httpd

Installed Complete!
```

**5. Edit the httpd.conf & change the Listen port from 80 to 3001**

```

[root@stbkp01 ~]# vi /etc/httpd/conf/httpd.conf
Add the vi editor -> Listen 3001

[root@stbkp01 ~]# cat /etc/httpd/conf/httpd.conf |grep 3001
Listen 3001
```

**6. Install the packages Nginx.**

```

[root@stbkp01 ~]# yum install nginx

[root@stbkp01 ~]# yum install epel-release

Installed Complete!
```

**7. Copy the index.html file from /tmp to /var/www/html/**

```

[root@stbkp01 ~]# cp /tmp/index.html /var/www/html/
[root@stbkp01 ~]# ls /var/www/html
index.html
```

**8. Edit the nginx.conf & change the Listen port from 80 to 8093**

```

[root@stbkp01 ~]# vi /etc/nginx/conf/

  server {
        listen       8093 default_server;
        listen       [::]:8093 default_server;
        server_name  172.16.238.16;
        root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        proxy_pass http://172.16.238.16:3001;
        }
```

**9. Run the command nginx to know the status**

```

root@stbkp01 ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

**10. Start the httpd and nginx services** 

```

[root@stbkp01 ~]# systemctl start httpd
[root@stbkp01 ~]# systemctl start nginx
[root@stbkp01 ~]# systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2023-09-20 06:18:37 UTC; 17s ago
  Process: 2516 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
  Process: 2503 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  Process: 2490 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)


[root@stbkp01 ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2023-09-20 06:18:28 UTC; 1min 50s ago
     Docs: man:httpd.service(8)
 Main PID: 2234 (httpd)
   Status: "Running, listening on: port 3001"
```

**11. validate the task by below commands**

```
	curl http://172.16.238.16:8093

	curl http://172.16.238.16:3001

[root@stbkp01 ~]# curl http://172.16.238.16:8093
Welcome to xFusionCorp Industries!
[root@stbkp01 ~]# 

[root@stbkp01 ~]# curl http://172.16.238.16:3001
Welcome to xFusionCorp Industries!
[root@stbkp01 ~]# 
```

**12. Click on `Finish` & `Confirm` to complete the task successful**