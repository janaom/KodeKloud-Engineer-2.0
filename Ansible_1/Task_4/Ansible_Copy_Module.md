## Instructions

There is data on `jump host` that needs to be copied on `all application servers` in `Stratos DC`.  Nautilus DevOps team want to perform this task using `Ansible`. Perform the task as per details mentioned below:

a. On `jump host` create an inventory file `/home/thor/ansible/inventory` and add all application servers as managed nodes.

b. On `jump host` create a playbook  `/home/thor/ansible/playbook.yml` to copy `/usr/src/data/index.html` file to all application servers at location `/opt/data`.

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.

# Solution

NOTE: if the task wants you to run ansible-playbook -i inventory playbook.yml don't create playbook.yaml (yml vs yaml)

Create inventory and playbook.yml

Run: ansible all -a "ls -ltr /opt/data" -i inventory (to check the inventory file is working correctly by listing folder on all the app servers)

Run: ansible-playbook -i inventory playbook.yml

Check the result: ansible all -a "ls -ltr /opt/data" -i inventory

OR: ssh into e.g. steve@stapp02.stratos.xfusioncorp.com and check /opt/data
