# Instructions

The Nautilus Application development team wanted to test some applications on app servers in `Stratos Datacenter`. They shared some pre-requisites with the DevOps team, and packages need to be installed on app servers. Since we are already using Ansible for automating such tasks, please perform this task using Ansible as per details mentioned below:

1. Create an inventory file `/home/thor/playbook/inventory` on `jump host` and add all app servers in it.
2. Create an Ansible playbook `/home/thor/playbook/playbook.yml` to install `wget` package on `all  app servers` using Ansible `yum` module.
3. Make sure user `thor` should be able to run the playbook on `jump host`.

`Note:` Validation will try to run playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.

# Solution

cd to /playbook

Create an inventory, add:
```
[app_servers]
stapp01 ansible_user=tony ansible_ssh_pass=Ir0nM@n
stapp02 ansible_user=steve ansible_ssh_pass=Am3ric@
stapp03 ansible_user=banner ansible_ssh_pass=BigGr33n
```

Create a playbook.yml
```YAML
---
- name: Install wget package
  hosts: app_servers
  become: true

  tasks:
    - name: Install wget package
      yum:
        name: wget
        state: present
```

The playbook consists of a single play named "Install wget package." It targets the hosts group `app_servers`, which you need to define in your inventory file. The `become: true` line enables privilege escalation, allowing Ansible to execute commands with administrative privileges.

Inside the play, there is a single task named "Install wget package." It uses the `yum` module to install the package. The `name` parameter specifies the package name (`wget`), and the `state` parameter is set to `present`, indicating that Ansible should ensure the package is installed.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/db13194e-4e9a-4178-88d2-4487b0fa3288)

Execute the playbook using the modified inventory file by running the following command: `ansible-playbook -i inventory playbook.yml`

To check if the `wget` package was installed on all hosts, you can use the `ansible` command with the `shell` module to run a command remotely and check the package's presence. Here's the command you can use: `ansible all -i inventory -m shell -a 'rpm -q wget'`

This command runs the `rpm -q wget` command on all hosts defined in your inventory file. The `rpm -q` command checks if the `wget` package is installed, and the output will indicate whether it is present or not.

When you execute this command, Ansible will connect to each host specified in the inventory and run the command remotely. The output will be displayed in the terminal, allowing you to see if the package is installed on each host.

If the package is installed, you will see the package version as output. If it is not installed, you will see an error message indicating that the package is not found.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/db29dea6-a070-42d8-922d-22fb799181a8)



