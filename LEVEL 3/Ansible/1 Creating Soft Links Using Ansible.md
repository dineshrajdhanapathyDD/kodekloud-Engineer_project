

## Questions:

The Nautilus DevOps team is practicing some of the Ansible modules and creating and testing different Ansible playbooks to accomplish tasks. Recently they started testing an Ansible file module to create soft links on all app servers. Below you can find more details about it.

Write a `playbook.yml` under `/home/thor/ansible` directory on `jump host`, an `inventory` file is already present under `/home/thor/ansible` directory on `jump host` itself. Using this playbook accomplish below given tasks:

1. Create an empty file `/opt/finance/blog.txt` on app server 1; its user owner and group owner should be `tony`. Create a `symbolic link` of source path `/opt/finance` to destination `/var/www/html`.

2. Create an empty file `/opt/finance/story.txt` on app server 2; its user owner and group owner should be `steve`. Create a `symbolic link` of source path `/opt/finance` to destination `/var/www/html`.

3. Create an empty file `/opt/finance/media.txt` on app server 3; its user owner and group owner should be `banner`. Create a `symbolic link` of source path `/opt/finance` to destination `/var/www/html`.

`Note`: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way without passing any extra arguments.



## Solution: 

**1. At first ansible inventory file is working properly and doesn't have any file exist from jump server to all the app server's**

```
thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/finance" -i inventory
stapp02 | CHANGED | rc=0 >>
total 0
stapp01 | CHANGED | rc=0 >>
total 0
stapp03 | CHANGED | rc=0 >>
total 0
thor@jump_host ~/ansible$ 
```

**2.  If you see above output then create playbook file `ansible-playbook -i inventory playbook.yml`**

```
thor@jump_host ~/ansible$ vi /home/thor/ansible/playbook.yml
thor@jump_host ~/ansible$ cat /home/thor/ansible/playbook.yml
- name: Create text files and create soft link

  hosts: stapp01, stapp02, stapp03

  become: yes

  tasks:

    - name: Create the blog.txt on stapp01

      file:

        path: /opt/finance/blog.txt

        owner: tony

        group: tony

        state: touch

      when: inventory_hostname == "stapp01"

    - name: Create the story.txt on stapp02

      file:

        path: /opt/finance/story.txt

        owner: steve

        group: steve

        state: touch

      when: inventory_hostname == "stapp02"

    - name: Create the media.txt on stapp03

      file:

        path: /opt/finance/media.txt

        owner: banner

        group: banner

        state: touch

      when: inventory_hostname == "stapp03"

    - name: Link /opt/finance directory

      file:

        src: /opt/finance/

        dest: /var/www/html

        state: link
thor@jump_host ~/ansible$ 
```

**3. Post file saved , run below command to execute the playbook**

```

thor@jump_host ~/ansible$ ansible-playbook  -i inventory playbook.yml

PLAY [Create text files and create soft link] ******************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Create the blog.txt on stapp01] **************************************************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Create the story.txt on stapp02] *************************************************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Create the media.txt on stapp03] *************************************************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

TASK [Link /opt/finance directory] *****************************************************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP *****************************************************************************************************************
stapp01                    : ok=3    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp02                    : ok=3    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   

thor@jump_host ~/ansible$ 
```

**4. Validate the task by below command**

```

thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/finance" -i inventory
stapp01 | CHANGED | rc=0 >>
total 0
-rw-r--r-- 1 tony tony 0 Oct  2 08:08 blog.txt
stapp03 | CHANGED | rc=0 >>
total 0
-rw-r--r-- 1 banner banner 0 Oct  2 08:08 media.txt
stapp02 | CHANGED | rc=0 >>
total 0
-rw-r--r-- 1 steve steve 0 Oct  2 08:08 story.txt
thor@jump_host ~/ansible$ 
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**