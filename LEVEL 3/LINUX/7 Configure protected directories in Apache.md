

## Questions:

`xFusionCorp Industries` has hosted several static websites on `Nautilus Application Servers` in `Stratos DC`. There are some confidential directories in the document root that need to be password protected. Since they are using `Apache` for hosting the websites, the production support team has decided to use `.htaccess` with basic `auth`. There is a website that needs to be uploaded to `/var/www/html/sysops` on `Nautilus App Server 2`. However, we need to set up the authentication before that.

1. Create `/var/www/html/sysops` direcotry if doesn't exist.

2. Add a user `anita` in `htpasswd` and set its password to `TmPcZjtRQx`.

3. There is a file `/tmp/index.html` present on `Jump Server`. Copy the same to the directory you created, please make sure default document root should remain `/var/www/html`. Also website should work on URL `http://<app-server-hostname>:8080/sysops/`

## Solution:

**1. At first  copy the index file from jump server to app server**

```

thor@jump_host ~$ scp /tmp/index.html steve@stapp02:/tmp
The authenticity of host 'stapp02 (172.16.238.11)' can't be established.
ECDSA key fingerprint is SHA256:l3iszU5lklfCuq1I+oG+ifVbW4d229eRlhyuomWnzJQ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp02,172.16.238.11' (ECDSA) to the list of known hosts.
steve@stapp02's password: 

```

**2.  Login on app server  ssh steve@stapp02**

```

thor@jump_host ~$ ssh steve@stapp02
steve@stapp02's password: Am3ric@
```

**3. Switch to  root user : sudo su -**

```

[steve@stapp02 ~]$ sudo su -
[sudo] password for steve: Am3ric@
[root@stapp02 ~]# 
```

**4. Run Below command to check the existing Apache httpd service status**

```

[root@stapp02 ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: man:httpd.service(8)

Sep 21 07:24:22 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Collecting.
```

**5. Create/var/www/html/sysops directory**

```
[root@stapp02 ~]# mkdir /var/www/html/sysops
```

**6. Add a user and set password as per given in your task** 

```

[root@stapp02 ~]# htpasswd -c /etc/httpd/.htpasswd  anita
New password: TmPcZjtRQx
Re-type new password: TmPcZjtRQx
Adding password for user anita
```

**7. create .htaccess file under dba directory**

```

[root@stapp02 ~]# vi  /var/www/html/sysops/.htaccess
[root@stapp02 ~]# cat /var/www/html/sysops/.htaccess
AuthType Basic
AuthName "Password Required"
Require valid-user
AuthUserFile /etc/httpd/.htpasswd
```

**8. Move the copied index.html file under dba directory**

```
[root@stapp02 ~]# mv /tmp/index.html  /var/www/html/sysops/
```

**9. Post saved config file , start  & check the httpd services status**

```

[root@stapp02 ~]# systemctl start httpd && systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Thu 2023-09-21 07:34:02 UTC; 100ms ago
     Docs: man:httpd.service(8)
 Main PID: 1089 (httpd)
   Status: "Started, listening on: port 8080"
    Tasks: 213 (limit: 1340692)
```

**10.  Validate Apache httpd running  as per the task request user and password**

```

[root@stapp02 ~]#  curl -u anita http://stapp02:8080/sysops
Enter host password for user 'anita':
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://stapp02:8080/sysops/">here</a>.</p>
</body></html>
```

**11. Click on `Finish` & `Confirm` to complete the task successful**