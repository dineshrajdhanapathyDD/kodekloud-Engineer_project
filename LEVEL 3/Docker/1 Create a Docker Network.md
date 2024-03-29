

## Questions:

The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:


a. Create a docker network named as `blog` on App Server 3 in `Stratos DC`.

b. Configure it to use `bridge` drivers.

c. Set it to use subnet `172.168.0.0/24` and iprange `172.168.0.3/24`.



## Solution:   

**1. Login on app server  as per the task  &  Switch to  root user**

```

thor@jump_host ~$ ssh banner@stapp03
The authenticity of host 'stapp03 (172.16.238.12)' can't be established.
ECDSA key fingerprint is SHA256:bzPKzq4tRnlw6xleOwJ0M2Ox6yW9oAhw4eKWP0jUFN4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.
banner@stapp03's password: BigGr33n
[banner@stapp03 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: BigGr33n
[root@stapp03 ~]# 
```

**2. Check existing network on server**

```

[root@stapp03 ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
06eb8353433a        bridge              bridge              local
2facff946852        host                host                local
daf4ba5e867d        none                null                local
```

**3. Create a docker network as per the task use network name subnet  & IPrange**

```

[root@stapp03 ~]# docker network create -d bridge --subnet=172.168.0.0/24 --ip-range=172.168.0.3/24 blog
1bbc29ceea3c5312faac954552a4098808ef700193dbb03213b43fb7818b1b7b
```

**4. Validate the task network has been created successfully** 

```

[root@stapp03 ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
1bbc29ceea3c        blog                bridge              local
06eb8353433a        bridge              bridge              local
2facff946852        host                host                local
daf4ba5e867d        none                null                local


[root@stapp03 ~]# docker network inspect blog
[
    {
        "Name": "blog",
        "Id": "1bbc29ceea3c5312faac954552a4098808ef700193dbb03213b43fb7818b1b7b",
        "Created": "2023-09-26T10:59:11.699499819Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.168.0.0/24",
                    "IPRange": "172.168.0.3/24"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
[root@stapp03 ~]# 
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**