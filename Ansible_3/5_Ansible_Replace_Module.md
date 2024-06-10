# Instructions

There is some data on all app servers in `Stratos DC`. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes they made. The DevOps team is working to prepare an Ansible playbook to accomplish the same. Below you can find more details about the task.

Write a `playbook.yml` under `/home/thor/ansible` on `jump host`, an `inventory` is already present under `/home/thor/ansible` directory on `Jump host` itself. Perform below given tasks using this playbook:

1. We have a file `/opt/itadmin/blog.txt` on app server 1. Using Ansible `replace` module replace string `xFusionCorp` to `Nautilus` in that file.
2. We have a file `/opt/itadmin/story.txt` on app server 2. Using Ansible`replace` module replace the string `Nautilus` to `KodeKloud` in that file.
3. We have a file `/opt/itadmin/media.txt` on app server 3. Using Ansible `replace` module replace string `KodeKloud` to `xFusionCorp Industries` in that file.

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.

# Solution

`cd /home/thor/ansible`

`vi playbook.yml`

```yaml
- name: Using Ansible Replace
  hosts: all
  become: yes
  tasks:

    - name: Replace text in blog.txt 
      replace:
        path: /opt/itadmin/blog.txt
        regexp: "xFusionCorp"
        replace: "Nautilus"
      when: inventory_hostname == "stapp01"

    - name: Replace text in story.txt 
      replace:
        path: /opt/itadmin/story.txt
        regexp: "Nautilus"
        replace: "KodeKloud"
      when: inventory_hostname == "stapp02"

    - name: Replace text in media.txt 
      replace:
        path: /opt/itadmin/media.txt
        regexp: "KodeKloud"
        replace: "xFusionCorp Industries"
      when: inventory_hostname == "stapp03"
```

`ansible-playbook -i inventory playbook.yml`

```bash
thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml 

PLAY [Using Ansible Replace] *********************************************************************************

TASK [Gathering Facts] ***************************************************************************************
ok: [stapp03]
ok: [stapp02]
ok: [stapp01]

TASK [Replace text in blog.txt] ******************************************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Replace text in story.txt] *****************************************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Replace text in media.txt] *****************************************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP ***************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
```

`ansible all -i inventory -m shell -a  "cat /opt/itadmin/*”`

```bash
thor@jumphost ~/ansible$ ansible all -i inventory -m shell -a  "cat /opt/itadmin/*"
stapp03 | CHANGED | rc=0 >>
Welcome to xFusionCorp Industries !
stapp01 | CHANGED | rc=0 >>
Welcome to Nautilus Industries !
stapp02 | CHANGED | rc=0 >>
Welcome to KodeKloud Group !
```
