# Instructions

The Nautilus DevOps team is trying to setup a simple Apache web server on all app servers in Stratos DC using Ansible. They also want to create a sample html page for now with some app specific data on it. Below you can find more details about the task.

You will find a valid inventory file `/home/thor/playbooks/inventory` on jump host (which we are using as an Ansible controller).

1. Create a playbook `index.yml` under `/home/thor/playbooks` directory on `jump host`. Using `blockinfile` Ansible module create a file `facts.txt` under `/root` directory on all app servers and add the following given block in it. You will need to enable facts gathering for this task.

```
Ansible managed node architecture is <architecture>
```

(You can obtain the system architecture from Ansible's gathered facts by using the correct Ansible variable while taking into account Jinja2 syntax)

1. Install `httpd` server on all apps. After that make a copy of `facts.txt` file as `index.html` under `/var/www/html` directory. Make sure to start `httpd` service after that.

`Note:` Do not create a separate role for this task, just add all of the changes in `index.yml` playbook.

# Solution

`cd /home/thor/playbooks`

`vi index.yml`

```yaml
---
- name: Install httpd etc
  hosts: all
  gather_facts: true
  become: yes
  become_method: sudo
  tasks:
    - name: Use blockinfile to create facts.txt
      blockinfile:
        create: yes
        path: /root/facts.txt
        block: |
          Ansible managed node architecture is {{ ansible_architecture }}

    - name: Install Apache
      package:
        name: httpd

    - name: Copy facts.txt to index.html
      shell: |
        cp /root/facts.txt /var/www/html/index.html

    - name: Ensure httpd is running
      systemd:
        name: httpd
        state: restarted
```

`ansible-playbook -i inventory index.yml`

Check the result

```bash
thor@jumphost ~$ curl http://stapp01
# BEGIN ANSIBLE MANAGED BLOCK
Ansible managed node architecture is x86_64
# END ANSIBLE MANAGED BLOCK
thor@jumphost ~$ curl http://stapp02
# BEGIN ANSIBLE MANAGED BLOCK
Ansible managed node architecture is x86_64
# END ANSIBLE MANAGED BLOCK
thor@jumphost ~$ curl http://stapp03
# BEGIN ANSIBLE MANAGED BLOCK
Ansible managed node architecture is x86_64
# END ANSIBLE MANAGED BLOCK
thor@jumphost ~$
```
