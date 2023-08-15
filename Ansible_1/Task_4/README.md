NOTE: if the task wants you to run ansible-playbook -i inventory playbook.yml don't create playbook.yaml (yml vs yaml)

Create inventory and playbook.yml

Run: ansible all -a "ls -ltr /opt/data" -i inventory (to check the inventory file is working correctly by listing folder on all the app servers)

Run: ansible-playbook -i inventory playbook.yml

Check the result: ansible all -a "ls -ltr /opt/data" -i inventory

OR: ssh into e.g. steve@stapp02.stratos.xfusioncorp.com and check /opt/data
