

## Questions:

Developers are looking for dependencies to be installed and run on Nautilus app servers in Stratos DC. They have shared some requirements with the DevOps team. Because we are now managing packages installation and services management using Ansible, some playbooks need to be created and tested. As per details mentioned below please complete the task:



a. On `jump host` create an Ansible playbook `/home/thor/ansible/playbook.yml` and configure it to install `vsftpd` on all app servers.


b. After installation make sure to start and enable `vsftpd`service on all app servers.


c. The inventory `/home/thor/ansible/inventory` is already there on `jump host`.


d. Make sure user `thor` should be able to run the playbook on `jump host`.


`Note`: Validation will try to run playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.


## Solution:  

**1. Go through the folder mentioned in task and create inventory & playbook files**   

```

thor@jump_host ~$ cd /home/thor/ansible/
thor@jump_host ~/ansible$ ls
inventory
thor@jump_host ~/ansible$ 
```

**2.  Create a playbook file as per the task**

```

thor@jump_host ~/ansible$ vi playbook.yml
thor@jump_host ~/ansible$ cat playbook.yml
- name: Install and configure httpd
  hosts: all
  become: true
  tasks:
    - name: Install vsftpd package
      yum:
        name: vsftpd
        state: present

    - name: Start vsftpd service
      systemd:
        name: vsftpd
        enabled: yes
        state: started
thor@jump_host ~/ansible$ 
```

**3. Run the playbook**

```

thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Install and configure httpd] *****************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp03]
ok: [stapp01]
ok: [stapp02]

TASK [Install vsftpd package] **********************************************************************************************
changed: [stapp01]
changed: [stapp03]
changed: [stapp02]

TASK [Start vsftpd service] ************************************************************************************************
changed: [stapp02]
changed: [stapp01]
changed: [stapp03]

PLAY RECAP *****************************************************************************************************************
stapp01                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

thor@jump_host ~/ansible$ 
```

**4.To verify, run the following**

```

thor@jump_host ~/ansible$ ansible all -i inventory -a "rpm -q vsftpd"
stapp01 | CHANGED | rc=0 >>
vsftpd-3.0.3-36.el8.x86_64
stapp03 | CHANGED | rc=0 >>
vsftpd-3.0.3-36.el8.x86_64
stapp02 | CHANGED | rc=0 >>
vsftpd-3.0.3-36.el8.x86_64
thor@jump_host ~/ansible$ 
```

**5. Status of vsftpd**

```

thor@jump_host ~/ansible$ ansible all -i inventory -a "sudo systemctl status vsftpd"
stapp03 | CHANGED | rc=0 >>
● vsftpd.service - Vsftpd ftp daemon
   Loaded: loaded (/usr/lib/systemd/system/vsftpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2023-10-03 13:00:22 UTC; 1min 38s ago
  Process: 1746 ExecStart=/usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf (code=exited, status=0/SUCCESS)
 Main PID: 1766 (vsftpd)
    Tasks: 1 (limit: 1340692)
   Memory: 732.0K
   CGroup: /docker/c0a28be0805bbcfcd371254f4356e7fe9d68326d690c5a5e823e274329515f08/system.slice/vsftpd.service
           └─1766 /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf

Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1]: Starting Vsftpd ftp daemon...
Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1746]: vsftpd.service: Executing: /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf
Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Child 1746 belongs to vsftpd.service.
Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Control process exited, code=exited status=0
Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Got final SIGCHLD for state start.
Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Main PID guessed: 1766
Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Changed start -> running
Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Job vsftpd.service/start finished, result=done
Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1]: Started Vsftpd ftp daemon.
Oct 03 13:00:22 stapp03.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Failed to send unit change signal for vsftpd.service: Connection reset by peer
stapp02 | CHANGED | rc=0 >>
● vsftpd.service - Vsftpd ftp daemon
   Loaded: loaded (/usr/lib/systemd/system/vsftpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2023-10-03 13:00:22 UTC; 1min 38s ago
  Process: 1747 ExecStart=/usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf (code=exited, status=0/SUCCESS)
 Main PID: 1767 (vsftpd)
    Tasks: 1 (limit: 1340692)
   Memory: 768.0K
   CGroup: /docker/a5d558126de78a90132def2431d1c6ef2d471e8c869d38ba1ee0a1b1ddb679ad/system.slice/vsftpd.service
           └─1767 /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf

Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1]: Starting Vsftpd ftp daemon...
Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1747]: vsftpd.service: Executing: /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf
Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Child 1747 belongs to vsftpd.service.
Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Control process exited, code=exited status=0
Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Got final SIGCHLD for state start.
Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Main PID guessed: 1767
Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Changed start -> running
Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Job vsftpd.service/start finished, result=done
Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1]: Started Vsftpd ftp daemon.
Oct 03 13:00:22 stapp02.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Failed to send unit change signal for vsftpd.service: Connection reset by peer
stapp01 | CHANGED | rc=0 >>
● vsftpd.service - Vsftpd ftp daemon
   Loaded: loaded (/usr/lib/systemd/system/vsftpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2023-10-03 13:00:22 UTC; 1min 38s ago
  Process: 1832 ExecStart=/usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf (code=exited, status=0/SUCCESS)
 Main PID: 1846 (vsftpd)
    Tasks: 1 (limit: 1340692)
   Memory: 748.0K
   CGroup: /docker/eca0312c9c16e104039bd2a0a7c3566fd152e7da79dca57fabf412c354a18afc/system.slice/vsftpd.service
           └─1846 /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf

Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1832]: PR_SET_MM_ARG_START failed, proceeding without: Operation not permitted
Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1832]: vsftpd.service: Executing: /usr/sbin/vsftpd /etc/vsftpd/vsftpd.conf
Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Child 1832 belongs to vsftpd.service.
Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Control process exited, code=exited status=0
Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Got final SIGCHLD for state start.
Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Main PID guessed: 1846
Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Changed start -> running
Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Job vsftpd.service/start finished, result=done
Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1]: Started Vsftpd ftp daemon.
Oct 03 13:00:22 stapp01.stratos.xfusioncorp.com systemd[1]: vsftpd.service: Failed to send unit change signal for vsftpd.service: Connection reset by peer
thor@jump_host ~/ansible$ 

thor@jump_host ~/ansible$ ansible all -i inventory -a "sudo systemctl start vsftpd"
stapp02 | CHANGED | rc=0 >>

stapp03 | CHANGED | rc=0 >>

stapp01 | CHANGED | rc=0 >>

thor@jump_host ~/ansible$ 

thor@jump_host ~/ansible$ ansible all -i inventory -a "sudo systemctl enable vsftpd"
stapp03 | CHANGED | rc=0 >>

stapp01 | CHANGED | rc=0 >>

stapp02 | CHANGED | rc=0 >>

thor@jump_host ~/ansible$ 
```

**6. Click on `Finish` & `Confirm` to complete the task successful**