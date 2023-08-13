thor@jump_host ~$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

```
#1) Respect the privacy of others.
#2) Think before you type.
#3) With great power comes great responsibility.

```

[sudo] password for thor:
[root@jump_host ~]# cat /etc/ansible/ansible.cfg |grep remote_user -B5

SSH timeout

#timeout = 10

default user to use for playbooks if user is not specified

(/usr/bin/ansible will use current user as default)

#remote_user = root
[root@jump_host ~]# vi /etc/ansible/ansible.cfg

#FIND THIS PART IN ansible.cfg

default user to use for playbooks if user is not specified

(/usr/bin/ansible will use current user as default)

#change remote_user to siva
remote_user = siva   

[root@jump_host ~]# cat /etc/ansible/ansible.cfg |grep remote_user -B5

SSH timeout

#timeout = 10

default user to use for playbooks if user is not specified

(/usr/bin/ansible will use current user as default)

remote_user = siva
