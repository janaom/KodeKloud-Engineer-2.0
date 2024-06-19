# Instructions

Several new developers and DevOps engineers just joined the xFusionCorp industries. They have been assigned the `Nautilus` project, and as per the onboarding process we need to create user accounts for new joinees on at least one of the app servers in `Stratos DC`. We also need to create groups and make new users members of those groups. We need to accomplish this task using Ansible. Below you can find more information about the task.

There is already an `inventory` file `~/playbooks/inventory` on `jump host`.

On `jump host` itself there is a list of users in `~/playbooks/data/users.yml` file and there are two groups — `admins` and `developers` —that have list of different users. Create a playbook `~/playbooks/add_users.yml` on `jump host` to perform the following tasks on `app server 1` in `Stratos DC`.

a. Add all users given in the `users.yml` file on `app server 1`.

b. Also add `developers` and `admins` groups on the same server.

c. As per the list given in the `users.yml` file, make each user member of the respective group they are listed under.

d. Make sure home directory for all of the users under `developers` group is `/var/www` (not the default i.e `/var/www/{USER}`). Users under `admins` group should use the default home directory (i.e `/home/devid` for user `devid`).

e. Set password `GyQkFRVNr3` for all of the users under `developers` group and `dCV3szSGNA` for of the users under `admins` group. Make sure to use the password given in the `~/playbooks/secrets/vault.txt` file as Ansible vault password to encrypt the original password strings. You can use `~/playbooks/secrets/vault.txt` file as a vault secret file while running the playbook (make necessary changes in `~/playbooks/ansible.cfg` file).

f. All users under `admins` group must be added as sudo users. To do so, simply make them member of the `wheel` group as well.

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory add_users.yml` so please make sure playbook works this way, without passing any extra arguments.

# Solution

`cat ~/playbooks/data/users.yml`

```bash
thor@jumphost ~$ cat ~/playbooks/data/users.yml
admins:
  - rob
  - david
  - joy

developers:
  - tim
  - ray
  - jim
  - mark
```

`cd playbooks/`

`cat secrets/vault.txt`

```bash
thor@jumphost ~/playbooks$ cat secrets/vault.txt
P@ss3or432
```

`vi ansible.cfg`

Added those lines

```bash
[defaults]
host_key_checking = False
inventory = ~/playbooks/inventory
vault_password_file = ~/playbooks/secrets/vault.txt
```

`vi add_users.yml`

```yaml
---

- name: Create Users and Groups on App Server 1
  hosts: stapp01
  become: yes
  tasks:
  - name: Create the Admin Group 
    group:
        name: admins 
        state: present
  - name: Create the Dev Group
    group:
        name: developers 
        state: present
  - name: Create the Users for Admin Group
    user:
        name: "{{ item }}"
        password: "{{ 'dCV3szSGNA' | password_hash ('sha512') }}"
        groups: admins, wheel 
        state: present 
    loop:
    - rob 
    - david 
    - joy         
  - name: Create the Users for Dev Group
    user:
        name: "{{ item }}"
        password: "{{ 'GyQkFRVNr3' | password_hash ('sha512') }}"
        groups: developers 
        home: "/var/www/{{ item }}"
        state: present 
    loop:
    - tim
    - ray 
    - jim
    - mark
```

`ansible-playbook add_users.yml`

```bash
thor@jumphost ~/playbooks$ ansible-playbook add_users.yml 

PLAY [Create Users and Groups on App Server 1] ******************************************************************************

TASK [Gathering Facts] ******************************************************************************************************
ok: [stapp01]

TASK [Create the Admin Group] ***********************************************************************************************
changed: [stapp01]

TASK [Create the Dev Group] *************************************************************************************************
changed: [stapp01]

TASK [Create the Users for Admin Group] *************************************************************************************
[DEPRECATION WARNING]: Encryption using the Python crypt module is deprecated. The Python crypt module is deprecated and 
will be removed from Python 3.13. Install the passlib library for continued encryption functionality. This feature will be 
removed in version 2.17. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
changed: [stapp01] => (item=rob)
changed: [stapp01] => (item=david)
changed: [stapp01] => (item=joy)

TASK [Create the Users for Dev Group] ***************************************************************************************
[DEPRECATION WARNING]: Encryption using the Python crypt module is deprecated. The Python crypt module is deprecated and 
will be removed from Python 3.13. Install the passlib library for continued encryption functionality. This feature will be 
removed in version 2.17. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
changed: [stapp01] => (item=tim)
changed: [stapp01] => (item=ray)
changed: [stapp01] => (item=jim)
changed: [stapp01] => (item=mark)

PLAY RECAP ******************************************************************************************************************
stapp01                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```

`ansible stapp01 -a "cat /etc/passwd”`

```bash
thor@jumphost ~/playbooks$ ansible stapp01 -a "cat /etc/passwd"
stapp01 | CHANGED | rc=0 >>
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/:/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
ansible:x:1000:1000::/home/ansible:/bin/bash
tony:x:1001:1001::/home/tony:/bin/bash
rob:x:1002:1004::/home/rob:/bin/bash
david:x:1003:1005::/home/david:/bin/bash
joy:x:1004:1006::/home/joy:/bin/bash
tim:x:1005:1007::/var/www/tim:/bin/bash
ray:x:1006:1008::/var/www/ray:/bin/bash
jim:x:1007:1009::/var/www/jim:/bin/bash
mark:x:1008:1010::/var/www/mark:/bin/bash
```

`ansible stapp01 -a "grep wheel /etc/group”`

```bash
thor@jumphost ~/playbooks$ ansible stapp01 -a "grep wheel /etc/group"
stapp01 | CHANGED | rc=0 >>
wheel:x:10:ansible,tony,rob,david,joy
```
