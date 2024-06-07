# Instructions

The Nautilus DevOps team is practicing some of the Ansible modules and creating and testing different Ansible playbooks to accomplish tasks. Recently they started testing an Ansible file module to create soft links on all app servers. Below you can find more details about it.

Write a `playbook.yml` under `/home/thor/ansible` directory on `jump host`, an `inventory` file is already present under `/home/thor/ansible` directory on `jump host` itself. Using this playbook accomplish below given tasks:

1. Create an empty file `/opt/itadmin/blog.txt` on app server 1; its user owner and group owner should be `tony`. Create a `symbolic link` of source path `/opt/itadmin` to destination `/var/www/html`.
2. Create an empty file `/opt/itadmin/story.txt` on app server 2; its user owner and group owner should be `steve`. Create a `symbolic link` of source path `/opt/itadmin` to destination `/var/www/html`.
3. Create an empty file `/opt/itadmin/media.txt` on app server 3; its user owner and group owner should be `banner`. Create a `symbolic link` of source path `/opt/itadmin` to destination `/var/www/html`.

`Note`: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way without passing any extra arguments.

# Solution

`cd /home/thor/ansible`

`vi playbook.yml`

```yaml
---

- hosts: stapp01
  gather_facts: false
  become: true 

  tasks:
    - name: Create empty file 
      file:
        path: /opt/itadmin/blog.txt
        state: touch
        owner: tony 
        group: tony
    - name: Create symbolic link 
      file:
        state: link
        src: /opt/itadmin
        dest: /var/www/html

- hosts: stapp02
  gather_facts: false
  become: true 

  tasks:
    - name: Create empty file 
      file:
        path: /opt/itadmin/story.txt
        state: touch
        owner: steve
        group: steve
    - name: Create symbolic link  
      file:
        state: link
        src: /opt/itadmin
        dest: /var/www/html

- hosts: stapp03
  gather_facts: false
  become: true 

  tasks:
    - name: Create empty file 
      file:
        path: /opt/itadmin/media.txt
        state: touch
        owner: banner 
        group: banner
    - name: Create symbolic link 
      file:
        state: link
        src: /opt/itadmin
        dest: /var/www/html
```

`ansible-playbook -i inventory playbook.yml`

```bash
PLAY RECAP ***************************************************************************************************
stapp01                    : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

`ansible all -a "ls -la /opt" -i inventory`

`ansible all -a "ls -la /opt/itadmin" -i inventory`

`ansible all -a "ls -la /var/www/html" -i inventory`

```bash
thor@jump_host ~/ansible$ ansible all -a "ls -la /opt" -i inventory
stapp01 | CHANGED | rc=0 >>
total 12
drwxr-xr-x 1 root root 4096 Jun  7 07:37 .
drwxr-xr-x 1 root root 4096 Jun  7 07:37 ..
drwxr-xr-x 2 root root 4096 Jun  7 07:43 itadmin
stapp03 | CHANGED | rc=0 >>
total 12
drwxr-xr-x 1 root root 4096 Jun  7 07:37 .
drwxr-xr-x 1 root root 4096 Jun  7 07:37 ..
drwxr-xr-x 2 root root 4096 Jun  7 07:43 itadmin
stapp02 | CHANGED | rc=0 >>
total 12
drwxr-xr-x 1 root root 4096 Jun  7 07:37 .
drwxr-xr-x 1 root root 4096 Jun  7 07:37 ..
drwxr-xr-x 2 root root 4096 Jun  7 07:43 itadmin
thor@jump_host ~/ansible$ ansible all -a "ls -la /opt/itadmin" -i inventory
stapp02 | CHANGED | rc=0 >>
total 8
drwxr-xr-x 2 root  root  4096 Jun  7 07:43 .
drwxr-xr-x 1 root  root  4096 Jun  7 07:37 ..
-rw-r--r-- 1 steve steve    0 Jun  7 07:43 story.txt
stapp01 | CHANGED | rc=0 >>
total 8
drwxr-xr-x 2 root root 4096 Jun  7 07:43 .
drwxr-xr-x 1 root root 4096 Jun  7 07:37 ..
-rw-r--r-- 1 tony tony    0 Jun  7 07:43 blog.txt
stapp03 | CHANGED | rc=0 >>
total 8
drwxr-xr-x 2 root   root   4096 Jun  7 07:43 .
drwxr-xr-x 1 root   root   4096 Jun  7 07:37 ..
-rw-r--r-- 1 banner banner    0 Jun  7 07:43 media.txt
thor@jump_host ~/ansible$ ansible all -a "ls -la /var/www/html" -i inventory
stapp01 | CHANGED | rc=0 >>
lrwxrwxrwx 1 root root 12 Jun  7 07:43 /var/www/html -> /opt/itadmin
stapp02 | CHANGED | rc=0 >>
lrwxrwxrwx 1 root root 12 Jun  7 07:43 /var/www/html -> /opt/itadmin
stapp03 | CHANGED | rc=0 >>
lrwxrwxrwx 1 root root 12 Jun  7 07:43 /var/www/html -> /opt/itadmin
```
