# Instructions

The Nautilus DevOps team has some data on each app server in `Stratos DC`that they want to copy to a different location. However, they want to create an archive of the data first, then they want to copy the same to a different location on the respective app server. Additionally, there are some specific requirements for each server. Perform the task using Ansible playbook as per requirements mentioned below:

Create a playbook named `playbook.yml` under `/home/thor/ansible` directory on `jump host`, an `inventory` file is already placed under `/home/thor/ansible/` directory on `Jump Server` itself.

1. Create an archive `games.tar.gz` (make sure archive format is `tar.gz`) of `/usr/src/data/` directory ( present on each app server ) and copy it to `/opt/data/` directory on all app servers. The user and group `owner` of archive `games.tar.gz` should be `tony` for `App Server 1`, `steve` for `App Server 2` and `banner` for `App Server 3`.

`Note:` Validation will try to run playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.

# Solution

Check hosts:
```
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

Create a playbook and run it with `ansible-playbook -i inventory playbook.yml`
```YAML
---
- name: Create and copy archive
  hosts: stapp01, stapp02, stapp03
  become: true

  tasks:
    - name: Create archive
      archive:
        path: /usr/src/data/
        dest: /opt/data/games.tar.gz
        format: gz
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        force_archive: yes
```

Check the results with `ansible all -a "ls -ltr /opt/data" -i inventory`

When you run this command, Ansible will connect to each host defined in the inventory file and execute the ls -ltr /opt/data command remotely. The output of the command will be displayed in the terminal, showing the files and directories in the /opt/data directory on each host.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/a0cff44e-9422-4bc4-a15c-a20419369de5)
