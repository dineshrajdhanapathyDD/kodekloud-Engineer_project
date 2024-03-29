

## Questions:
One of the Nautilus DevOps team members was working on to test an Ansible playbook on `jump host`. However, he was only able to create the inventory, and due to other priorities that came in he has to work on other tasks. Please pick up this task from where he left off and complete it. Below are more details about the task:

The inventory file `/home/thor/ansible/inventory` seems to be having some issues, please fix them. The playbook needs to be run on `App Server 1` in (Stratos DC), so inventory file needs to be updated accordingly.

Create a playbook `/home/thor/ansible/playbook.yml` and add a task to create an empty file `/tmp/file.txt` on `App Server 1`.

Note: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.


## Solution:  

**1. Create a inventory file and add the app server as per your task**

```
thor@jump_host ~$ vi /home/thor/ansible/inventory

open new terminal 
paste ths command

stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n
control+c exit :wq

thor@jump_host ~$ cat /home/thor/ansible/inventory
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n
```

**2. Create a Playbook file**

```
thor@jump_host ~$ vi /home/thor/ansible/playbook.yml
open new terminal 
paste the command

- name: Create file in appserver
  hosts: stapp01
  become: yes
  tasks:
    - name: Create the file
      file:
        path: /tmp/file.txt
        state: touch
exit:wq

thor@jump_host ~$ cat /home/thor/ansible/playbook.yml
- name: Create file in appserver

  hosts: stapp01
  become: yes
  tasks:
    - name: Create the file
      file:
        path: /tmp/file.txt
        state: touch
```

**3.  Post file saved, run below command to execute the playbook**

```
thor@jump_host ~$ cd ansible
thor@jump_host ~/ansible$ ansible all -a "ls -ltr /tmp/" -i inventory
stapp01 | CHANGED | rc=0 >>
total 12
-rwx------ 1 root root  701 Feb  7  2023 ks-script-l36mq90h
-rwx------ 1 root root  291 Feb  7  2023 ks-script-kzk1nzfd
drwx------ 2 tony tony 4096 Aug 10 14:16 ansible_command_payload_fk5z326v

thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Create file in appserver] ********************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp01]

TASK [Create the file] *****************************************************************************************************
changed: [stapp01]

PLAY RECAP *****************************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

thor@jump_host ~/ansible$ ansible all -a "ls -ltr /tmp/" -i inventory
stapp01 | CHANGED | rc=0 >>
total 12
-rwx------ 1 root root  701 Feb  7  2023 ks-script-l36mq90h
-rwx------ 1 root root  291 Feb  7  2023 ks-script-kzk1nzfd
-rw-r--r-- 1 root root    0 Aug 10 14:20 file.txt
drwx------ 2 tony tony 4096 Aug 10 14:20 ansible_command_payload_yjpjmd9f
```

**4.  Click on `Finish` & `Confirm` to complete the task successfully**


