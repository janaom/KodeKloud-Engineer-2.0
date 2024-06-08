# Instructions

There are some files that need to be created on all app servers in `Stratos DC`. The Nautilus DevOps team want these files to be owned by user `root` only however, they also want that the app specific user to have a set of permissions on these files. All tasks must be done using Ansible only, so they need to create a playbook. Below you can find more information about the task.

Create a playbook named `playbook.yml` under `/home/thor/ansible` directory on `jump host`, an `inventory` file is already present under `/home/thor/ansible` directory on `Jump Server` itself.

1. Create an empty file `blog.txt` under `/opt/itadmin/` directory on app server 1. Set some `acl` properties for this file. Using `acl` provide `read '(r)'` permissions to `group tony` (i.e `entity` is `tony` and `etype` is `group`).
2. Create an empty file `story.txt` under `/opt/itadmin/` directory on app server 2. Set some `acl` properties for this file. Using `acl` provide `read + write '(rw)'` permissions to `user steve` (i.e `entity` is `steve` and `etype` is `user`).
3. Create an empty file `media.txt` under `/opt/itadmin/` on app server 3. Set some `acl` properties for this file. Using `acl` provide `read + write '(rw)'` permissions to `group banner` (i.e `entity` is `banner` and `etype` is `group`).

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way, without passing any extra arguments.

# Solution

`cd /home/thor/ansible`

`cat inventory`

```bash
thor@jump_host ~/ansible$ cat inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

Double check if inventory is correct: `ansible -m ping all -i inventory`

```bash
thor@jump_host ~/ansible$ ansible -m ping all -i inventory 
stapp03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
stapp01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
stapp02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
thor@jump_host ~/ansible$
```

`vi playbook.yml`

```yaml
- name: Create file and set ACL in Host 1
  hosts: stapp01
  become: yes
  tasks:
    - name: Create the blog.txt on stapp01
      file:
        path: /opt/itadmin/blog.txt
        state: touch
    - name: Set ACL for blog.txt
      acl:
        path: /opt/itadmin/blog.txt
        entity: tony
        etype: group
        permissions: r
        state: present

- name: Create file and set ACL in Host 2
  hosts: stapp02
  become: yes
  tasks:
    - name: Create the story.txt on stapp02
      file:
        path: /opt/itadmin/story.txt
        state: touch
    - name: Set ACL for story.txt
      acl:
        path: /opt/itadmin/story.txt
        entity: steve
        etype: user
        permissions: rw
        state: present

- name: Create file and set ACL in Host 3
  hosts: stapp03
  become: yes
  tasks:
    - name: Create the media.txt on stapp03
      file:
        path: /opt/itadmin/media.txt
        state: touch
    - name: Set ACL for media.txt
      acl:
        path: /opt/itadmin/media.txt
        entity: banner
        etype: group
        permissions: rw
        state: present  
```

`ansible-playbook  -i inventory playbook.yml`

```bash
thor@jump_host ~/ansible$ ansible-playbook  -i inventory playbook.yml  

PLAY [Create file and set ACL in Host 1] ***************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************
ok: [stapp01]

TASK [Create the blog.txt on stapp01] ******************************************************************************************************************
changed: [stapp01]

TASK [Set ACL for blog.txt] ****************************************************************************************************************************
changed: [stapp01]

PLAY [Create file and set ACL in Host 2] ***************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************
ok: [stapp02]

TASK [Create the story.txt on stapp02] *****************************************************************************************************************
changed: [stapp02]

TASK [Set ACL for story.txt] ***************************************************************************************************************************
changed: [stapp02]

PLAY [Create file and set ACL in Host 3] ***************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************
ok: [stapp03]

TASK [Create the media.txt on stapp03] *****************************************************************************************************************
changed: [stapp03]

TASK [Set ACL for media.txt] ***************************************************************************************************************************
changed: [stapp03]

PLAY RECAP *********************************************************************************************************************************************
stapp01                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

`ansible all -a "ls -ltr /opt/itadmin/" -i inventory`

```bash
thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/itadmin/" -i inventory
stapp02 | CHANGED | rc=0 >>
total 0
-rw-rw-r--+ 1 root root 0 Jun  8 16:27 story.txt
stapp03 | CHANGED | rc=0 >>
total 0
-rw-rw-r--+ 1 root root 0 Jun  8 16:27 media.txt
stapp01 | CHANGED | rc=0 >>
total 0
-rw-r--r--+ 1 root root 0 Jun  8 16:27 blog.txt
```
