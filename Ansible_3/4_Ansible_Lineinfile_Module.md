# Instructions

The Nautilus DevOps team want to install and set up a simple `httpd` web server on all app servers in `Stratos DC`. They also want to deploy a sample web page using Ansible. Therefore, write the required playbook to complete this task as per details mentioned below.

We already have an `inventory` file under `/home/thor/ansible` directory on `jump host`. Write a playbook `playbook.yml` under `/home/thor/ansible` directory on `jump host` itself. Using the playbook perform below given tasks:

1. Install `httpd` web server on all app servers, and make sure its service is up and running.
2. Create a file `/var/www/html/index.html` with content:

`This is a Nautilus sample file, created using Ansible!`

1. Using `lineinfile` Ansible module add some more content in `/var/www/html/index.html` file. Below is the content:

`Welcome to Nautilus Group!`

Also make sure this new line is added at the top of the file.

1. The `/var/www/html/index.html` file's user and group `owner` should be `apache` on all app servers.
2. The `/var/www/html/index.html` file's permissions should be `0777` on all app servers.

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.

# Solution

`cd /home/thor/ansible`

Check if inventory is fine: `ansible -m ping all -i inventory`

```bash
thor@jumphost ~/ansible$ ansible -m ping all -i inventory
stapp02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
stapp03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
stapp01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
thor@jumphost ~/ansible$
```

`vi playbook.yml`

```yaml
---
- name: Install and Configure Apache Web Server on all Apps
  hosts: all
  become: yes
  become_user: root
  
  tasks:
  - name: Install apache web server
    yum:
        name: httpd
        state: present

  - name: Start apache web service
    service:
        name: httpd
        state: started

  - name: Create the index.html file
    copy:
        dest: /var/www/html/index.html
        content: |
            This is a Nautilus sample file, created using Ansible!

  - name: Insert line at the top of the file
    lineinfile:
        path: /var/www/html/index.html
        line: "Welcome to Nautilus Group!"
        insertbefore: BOF

  - name: Set the file permissions and owner to 'apache'
    file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode: "0777"
```

`ansible-playbook -i inventory playbook.yml`

Check the result:

`sshpass -p  'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10`

`cat /var/www/html/index.html`

```bash
thor@jumphost ~/ansible$ sshpass -p  'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10
Last login: Mon Jun 10 12:14:50 2024 from 172.16.238.2
[tony@stapp01 ~]$ cat /var/www/html/index.html
Welcome to Nautilus Group!
This is a Nautilus sample file, created using Ansible!
```
`sudo systemctl status httpd`
