## Instructions

One of the Nautilus DevOps team members was working on to test an Ansible playbook on `jump host`. However, he was only able to create the inventory, and due to other 
priorities that came in he has to work on other tasks. Please pick up this task from where he left off and complete it. Below are more details about the task:

1. The inventory file `/home/thor/ansible/inventory` seems to be having some issues, please fix them. The playbook needs to be run on `App Server 2` in `Stratos DC`, so inventory file needs to be updated accordingly.
2. Create a playbook `/home/thor/ansible/playbook.yml` and add a task to create an empty file `/tmp/file.txt` on `App Server 2`.

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments

## Check the result

Double check the result: ssh steve@stapp02.stratos.xfusioncorp.com and check if there is an empty file.txt in /tmp.
