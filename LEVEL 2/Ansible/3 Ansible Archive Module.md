

## Questions:

The Nautilus DevOps team has some data on each app server in `Stratos DC` that they want to copy to a different location. However, they want to create an archive of the data first, then they want to copy the same to a different location on the respective app server. Additionally, there are some specific requirements for each server. Perform the task using Ansible playbook as per requirements mentioned below:



Create a playbook named `playbook.yml` under `/home/thor/ansible` directory on `jump host`, an `inventory` file is already placed under `/home/thor/ansible/` directory on `Jump Server` itself.


1. Create an archive `cluster.tar.gz` (make sure archive format is `tar.gz`) of `/usr/src/finance/` directory ` present on each app server ` and copy it to `/opt/finance/` directory on all app servers. The user and group `owner` of archive `cluster.tar.gz` should be `tony` for `App Server 1`, `steve` for `App Server 2` and `banner` for `App Server 3`.

`Note`: Validation will try to run playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.


## Solution:

**1. At first ansible inventory file is working properly and doesn't have any file exist from jump server to all the app server's**  

```

thor@jump_host ~$ cd /home/thor/ansible/
thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/finance/" -i inventory
stapp01 | CHANGED | rc=0 >>
total 0
stapp03 | CHANGED | rc=0 >>
total 0
stapp02 | CHANGED | rc=0 >>
total 0
```

**2.  If you see above output then create playbook file**

```

thor@jump_host ~/ansible$ vi /home/thor/ansible/playbook.yml
thor@jump_host ~/ansible$ cat /home/thor/ansible/playbook.yml
- name: Task create archive and copy to host

  hosts: stapp01, stapp02, stapp03

  become: yes

  tasks:

    - name: As per the task create the archive file and set the owner

      archive:

        path: /usr/src/finance/

        dest: /opt/finance/cluster.tar.gz

        format: gz

        force_archive: true

        owner: "{{ ansible_user }}"

        group: "{{ ansible_user }}"
```

**3.  Run below command to execute the task**

```

thor@jump_host ~/ansible$ ansible-playbook  -i inventory playbook.yml

PLAY [Task create archive and copy to host] **************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************
ok: [stapp02]
ok: [stapp03]
ok: [stapp01]

TASK [As per the task create the archive file and set the owner] *****************************************************************************************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP ***********************************************************************************************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```

**4.  Validate the task by below command**

```

thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/finance" -i inventory
stapp01 | CHANGED | rc=0 >>
total 4
-rw-r--r-- 1 tony tony 168 Sep 10 10:04 cluster.tar.gz
stapp02 | CHANGED | rc=0 >>
total 4
-rw-r--r-- 1 steve steve 176 Sep 10 10:04 cluster.tar.gz
stapp03 | CHANGED | rc=0 >>
total 4
-rw-r--r-- 1 banner banner 165 Sep 10 10:04 cluster.tar.gz
```

**5.  Click on `Finish` & `Confirm` to complete the task successful.**