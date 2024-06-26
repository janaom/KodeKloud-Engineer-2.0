# Instructions

The Nautilus DevOps team had a discussion about, how they can train different team members to use Ansible for different automation tasks. There are numerous ways to perform a particular task using Ansible, but we want to utilize each aspect that Ansible offers. The team wants to utilise Ansible's conditionals to perform the following task:

An `inventory` file is already placed under `/home/thor/ansible` directory on `jump host`, with all the `Stratos DC app servers` included.

Create a playbook `/home/thor/ansible/playbook.yml` and make sure to use Ansible's `when` conditionals statements to perform the below given tasks.

1. Copy `blog.txt` file present under `/usr/src/itadmin` directory on `jump host` to `App Server 1` under `/opt/itadmin` directory. Its user and group owner must be user `tony` and its permissions must be `0655` .
2. Copy `story.txt` file present under `/usr/src/itadmin` directory on `jump host` to `App Server 2` under `/opt/itadmin` directory. Its user and group owner must be user `steve` and its permissions must be `0655` .
3. Copy `media.txt` file present under `/usr/src/itadmin` directory on `jump host` to `App Server 3` under `/opt/itadmin` directory. Its user and group owner must be user `banner` and its permissions must be `0655`.

`NOTE:` You can use `ansible_nodename` variable from gathered facts with `when` condition. Additionally, please make sure you are running the play for all hosts i.e use `- hosts: all`.

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml`, so please make sure the playbook works this way without passing any extra arguments.

# Solution

`cd ansible/`

`cat inventory`

```bash
thor@jumphost ~/ansible$ cat inventory 
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

`ls -la /usr/src/itadmin`

```bash
thor@jumphost ~/ansible$ ls -la /usr/src/itadmin
total 20
drwxr-xr-x 2 root root 4096 Jun 26 09:01 .
drwxr-xr-x 1 root root 4096 Jun 26 09:01 ..
-rw-r--r-- 1 root root   35 Jun 26 09:00 blog.txt
-rw-r--r-- 1 root root   22 Jun 26 09:00 media.txt
-rw-r--r-- 1 root root   27 Jun 26 09:00 story.txt
```

`vi playbook.yml`

```yaml
- name: Copy the files to App Servers
  hosts: all
  become: yes
  tasks:
    - name: Copy blog.txt to stapp01
      ansible.builtin.copy:
        src: /usr/src/itadmin/blog.txt
        dest: /opt/itadmin
        owner: tony
        group: tony
        mode: "0655"
      when: inventory_hostname == "stapp01"

    - name: Copy story.txt to stapp02
      ansible.builtin.copy:
        src: /usr/src/itadmin/story.txt
        dest: /opt/itadmin
        owner: steve
        group: steve
        mode: "0655"
      when: inventory_hostname == "stapp02"

    - name: Copy media.txt to stapp03
      ansible.builtin.copy:
        src: /usr/src/itadmin/media.txt
        dest: /opt/itadmin
        owner: banner
        group: banner
        mode: "0655"
      when: inventory_hostname == "stapp03"
 ```

`ansible-playbook -i inventory playbook.yml`

```bash
thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Copy the files to App Servers] ****************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************
ok: [stapp03]
ok: [stapp02]
ok: [stapp01]

TASK [Copy blog.txt to stapp01] *********************************************************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Copy story.txt to stapp02] ********************************************************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Copy media.txt to stapp03] ********************************************************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP ******************************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
```

`ansible all -a "ls -lart /opt/itadmin" -i inventory`

```bash
thor@jumphost ~/ansible$ ansible all -a "ls -lart /opt/itadmin" -i inventory
stapp01 | CHANGED | rc=0 >>
total 12
drwxr-xr-x 1 root root 4096 Jun 26 09:01 ..
-rw-r-xr-x 1 tony tony   35 Jun 26 09:08 blog.txt
drwxr-xr-x 2 root root 4096 Jun 26 09:08 .
stapp03 | CHANGED | rc=0 >>
total 12
drwxr-xr-x 1 root   root   4096 Jun 26 09:01 ..
-rw-r-xr-x 1 banner banner   22 Jun 26 09:08 media.txt
drwxr-xr-x 2 root   root   4096 Jun 26 09:08 .
stapp02 | CHANGED | rc=0 >>
total 12
drwxr-xr-x 1 root  root  4096 Jun 26 09:01 ..
-rw-r-xr-x 1 steve steve   27 Jun 26 09:08 story.txt
drwxr-xr-x 2 root  root  4096 Jun 26 09:08 .
```
