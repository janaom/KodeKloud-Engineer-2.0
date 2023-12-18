# Instructions

The Nautilus DevOps team is planning to test several Ansible playbooks on different app servers in `Stratos DC`. Before that, some pre-requisites must be met. Essentially, the team needs to set up a password-less SSH connection between Ansible controller and Ansible managed nodes. One of the tickets is assigned to you; please complete the task as per details mentioned below:

a. `Jump host` is our Ansible controller, and we are going to run Ansible playbooks through `thor` user from `jump host`.

b. There is an inventory file `/home/thor/ansible/inventory` on `jump host`. Using that inventory file test `Ansible ping` from `jump host` to `App Server 1`, make sure ping works.

# Solution

Check inventory file

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/30ded6e2-d764-49c6-b8aa-e9c5e0f64ea8)

Generate SSH key pairs: `ssh-keygen`

This command is used to copy the public key to a remote server, enabling passwordless SSH authentication with the specified user (tony) and host (stapp01): `ssh-copy-id tony@stapp01`

Then change the inventory: `stapp01 ansible_user=tony ansible_ssh_private_key_file=~/.ssh/id_rsa`

And then try to ping: `ansible stapp01 -m ping -i inventory -v` This command runs an Ansible ad-hoc command to ping the stapp01 host. The -m option specifies the module to use, in this case, ping. The -i option is used to specify the inventory file (inventory). The -v option enables verbose mode, providing more detailed output during the execution of the command.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/f60e7f91-c3e5-4a50-8181-54a6b9afcf60)

If you are still lost try to read this [discussion](https://kodekloud.com/community/t/issue-with-the-task-named-ansible-ping-module-usage/353439)
