

## Questions:

Our monitoring tool has reported an issue in `(Stratos Datacenter)`. One of our app servers has an issue, as its Apache service is not reachable on port `3004 which is the Apache port`. The service itself could be down, the firewall could be at fault, or something else could be causing the issue.



Use tools like `telnet`, `netstat`, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command `curl http://stapp01:3004` command from jump host.



## Solution:

**1. At first  telnet from jump server to identify which App server have the issue**

```
thor@jump_host ~$ telnet stapp01 3004
Trying 172.16.238.10...
telnet: connect to address 172.16.238.10: No route to host
```

**2.  Cant able to telnet stapp01 so login on  server to troubleshoot further ssh tony@stapp01**

```

thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:TaYu/UH5bzGcrRoqCe2uq9afC+QqoNM5GXP186Aveq8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01,172.16.238.10' (ECDSA) to the list of known hosts.
tony@stapp01's password: Ir0nM@n
[tony@stapp01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: Ir0nM@n
```

**3.  Run Below command to check the existing Apache httpd service status & start, while start you may get below errors**

```

[root@stapp01 ~]# systemctl start httpd
Job for httpd.service failed because the control process exited with error code.
See "systemctl status httpd.service" and "journalctl -xe" for details.

[root@stapp01 ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since Tue 2023-09-19 13:16:09 UTC; 5s ago
     Docs: man:httpd.service(8)
  Process: 873 ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND (code=exited, status=1/FAILURE)
 Main PID: 873 (code=exited, status=1/FAILURE)
   Status: "Reading configuration..."

Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com httpd[873]: (98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:3004
Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com httpd[873]: no listening sockets available, shutting down
Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com httpd[873]: AH00015: Unable to open logs
Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Child 873 belongs to httpd.service.
Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Main process exited, code=exited, status=1/FAILURE
Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Failed with result 'exit-code'.
Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Changed start -> failed
Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Job httpd.service/start finished, result=failed
Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com systemd[1]: Failed to start The Apache HTTP Server.
Sep 19 13:16:09 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Unit entered failed state.
```

**4. With the help of  netstat check which service is using your port and whats is PID.**

```

[root@stapp01 ~]# netstat -tulnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.11:40519        0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      486/sshd            
tcp        0      0 127.0.0.1:3004          0.0.0.0:*               LISTEN      630/sendmail: accep 
tcp6       0      0 :::22                   :::*                    LISTEN      486/sshd            
udp        0      0 127.0.0.11:45200        0.0.0.0:*                           -     
```

**5. Kill the PID**

```

[root@stapp01 ~]# ps -ef |grep 630
root         630       1  0 13:04 ?        00:00:00 sendmail: accepting connections
root        1003     809  0 13:24 pts/0    00:00:00 grep --color=auto 630
[root@stapp01 ~]# kill 630
[root@stapp01 ~]# ps -ef |grep 630
root        1036     809  0 13:25 pts/0    00:00:00 grep --color=auto 630

[root@stapp01 ~]# netstat -tulnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.11:40519        0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      486/sshd            
tcp6       0      0 :::22                   :::*                    LISTEN      486/sshd            
udp        0      0 127.0.0.11:45200        0.0.0.0:*                           -           
```

**6. Validate Apache httpd running  as per the task request**

```

[root@stapp01 ~]# systemctl start httpd
[root@stapp01 ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Tue 2023-09-19 13:26:20 UTC; 9s ago
     Docs: man:httpd.service(8)
 Main PID: 1086 (httpd)
   Status: "Running, listening on: port 3004"
```

**7. With the help of  netstat check which service**

```

[root@stapp01 ~]# netstat -tulnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.11:40519        0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      486/sshd            
tcp        0      0 0.0.0.0:3004            0.0.0.0:*               LISTEN      1086/httpd          
tcp6       0      0 :::22                   :::*                    LISTEN      486/sshd            
udp        0      0 127.0.0.11:45200        0.0.0.0:*       
                    -      
```

**8. Run the command httpd to know the status**

```

[root@stapp01 ~]# httpd -t
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using stapp01.stratos.xfusioncorp.com. Set the 'ServerName' directive globally to suppress this message
Syntax OK
```

**9. Insert the servername , IP and port in the vi editor**
 
```

[root@stapp01 ~]# vi /etc/httpd/conf/httpd.conf

vi editor changes: 
servername 172.16.238.10:3004
```

**10. Restart the apache httpd**

```

[root@stapp01 ~]# systemctl restart httpd
[root@stapp01 ~]# httpd -t
Syntax OK


[root@stapp01 ~]# netstat -tulnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.11:40519        0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      486/sshd            
tcp        0      0 0.0.0.0:3004            0.0.0.0:*               LISTEN      1462/httpd          
tcp6       0      0 :::22                   :::*                    LISTEN      486/sshd            
udp        0      0 127.0.0.11:45200        0.0.0.0:*                           -                   
[root@stapp01 ~]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere             state RELATED,ESTABLISHED
ACCEPT     icmp --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere            
ACCEPT     tcp  --  anywhere             anywhere             state NEW tcp dpt:ssh
REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         
REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
# Warning: iptables-legacy tables present, use iptables-legacy to see them

[root@stapp01 ~]# iptables -F
```

**11. check the telnet to identify the app server connection**

```

thor@jump_host ~$ telnet stapp01 3004
Trying 172.16.238.10...
connected to stapp01
```

**12. Click on `Finish` & `Confirm` to complete the task successful**