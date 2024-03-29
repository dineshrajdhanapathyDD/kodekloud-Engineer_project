

## Questions:

Nautilus DevOps team is testing some applications deployment on some of the application servers. They need to deploy a nginx container on `Application Server 3`. Please complete the task as per details given below:


On `Application Server 3` create a container named `nginx_3` using image `nginx` with `alpine` tag and make sure container is in `running` state.

## Solutions:

**1. Login on app server given in task and switch the root.**

```
thor@jump_host ~$ ssh banner@stapp03
banner@stapp03's password: BigGr33n 

[banner@stapp03 ~]$ sudo su -
[sudo] password for banner: BigGr33n 


[root@stapp02 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

[root@stapp02 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

**2. As per task pull the image and run the container**

```
[root@stapp02 ~]# docker run -d --name nginx_2 -p 8080:80 nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
4db1b89c0bd1: Pull complete 
bd338968799f: Pull complete 
6a107772494d: Pull complete 
9f05b0cc5f6e: Pull complete 
4c5efdb87c4a: Pull complete 
c8794a7158bf: Pull complete 
8de2a93581dc: Pull complete 
768e67c521a9: Pull complete 
Digest: sha256:2d194184b067db3598771b4cf326cfe6ad5051937ba1132b8b7d4b0184e0d0a6
Status: Downloaded newer image for nginx:alpine
fd955b76cffb15c4bd8750c1c938abebe4ce364ab8adfe652b65b6d1194c9c54
```

**3. Confirm docker container is running**

```
[root@stapp02 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                  NAMES
fd955b76cffb        nginx:alpine        "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp   nginx_2

[root@stapp02 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               alpine              4937520ae206        6 weeks ago         41.4MB
```

**4. Run the docker in locallhost**

```
[root@stapp02 ~]# curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

**5.  Click on `Finish` & `Confirm` to complete the task successfully**

