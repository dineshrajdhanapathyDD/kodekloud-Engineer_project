

## Questions:
To manage all servers within the stack using Ansible, the Nautilus DevOps team is planning to use a common sudo user among all servers. Ansible will be able to use this to perform different tasks on each server. This is not finalized yet, but the team has decided to first perform testing. The DevOps team has already installed Ansible on jump host using yum, and they now have the following requirement:



On jump host make appropriate changes so that Ansible can use `ammar` as a default ssh user for all hosts. Make changes in Ansible's default configuration only —please do not try to create a new config.


## Solution:  

**1. Switch to Root user on jump server itself**

```
thor@jump_host /$ sudo su -

[sudo] password for thor: mjolnir123
```

**2. Go through the ansible configuration file and add remote user as per the task**

```
[root@jump_host ~]# cat /etc/ansible/ansible.cfg |grep remote_user -B5
# SSH timeout
#timeout = 10

# default user to use for playbooks if user is not specified
# (/usr/bin/ansible will use current user as default)
#remote_user = root

[root@jump_host ~]# vi /etc/ansible/ansible.cfg
 open new terminal:
# default user to use for playbooks if user is not specified
# (/usr/bin/ansible will use current user as default)
#remote_user = root

edit like this:
# default user to use for playbooks if user is not specified
# (/usr/bin/ansible will use current user as default)
remote_user = ammar

exit :wq


[root@jump_host ~]# cat /etc/ansible/ansible.cfg |grep remote_user -B5
# SSH timeout
#timeout = 10

# default user to use for playbooks if user is not specified
# (/usr/bin/ansible will use current user as default)
remote_user = ammar
```

**3. Click on `Finish` & `Confirm` to complete the task successful**
 