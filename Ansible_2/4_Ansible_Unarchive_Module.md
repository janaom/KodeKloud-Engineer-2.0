# Instructions

One of the DevOps team members has created a zip archive on `jump host` in `Stratos DC` that needs to be extracted and copied over to all app servers in `Stratos DC` itself. Because this is a routine task, the `Nautilus`DevOps team has suggested automating it. We can use Ansible since we have been using it for other automation tasks. Below you can find more details about the task:

We have an `inventory` file under `/home/thor/ansible` directory on `jump host`, which should have all the app servers added already.

There is a zip archive `/usr/src/finance/nautilus.zip` on `jump host`.

Create a `playbook.yml` under `/home/thor/ansible/` directory on `jump host` itself to perform the below given tasks.

1. Unzip `/usr/src/finance/nautilus.zip` archive in `/opt/finance/` location on all app  servers.
2. Make sure the extracted data must has the respective sudo user as their `user` and `group` owner, i.e tony for app server 1, steve for app server 2, banner for app server 3.
3. The extracted data permissions must be `0755`.

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.

# Solution

Check inventory file:
```
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

Create a playbook.yml and run `ansible-playbook -i inventory playbook.yml`
```YAML
---
- name: Unzip nautilus.zip archive
  hosts: stapp01, stapp02, stapp03
  become: true
  gather_facts: false

  tasks:
    - name: Unzip nautilus.zip archive
      unarchive:
        src: /usr/src/finance/nautilus.zip
        dest: /opt/finance/
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        mode: "0755"
```

Check the results: `ansible all -a "ls -ltr /opt/finance/" -i inventory`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/84d0cd2b-ae6f-4f14-ada4-9357b3a653d6)
