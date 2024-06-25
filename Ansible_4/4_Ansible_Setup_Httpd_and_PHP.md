# Instructions

Nautilus Application development team wants to test the Apache and PHP setup on one of the app servers in `Stratos Datacenter`. They want the DevOps team to prepare an Ansible playbook to accomplish this task. Below you can find more details about the task.

There is an inventory file `~/playbooks/inventory` on `jump host`.

Create a playbook `~/playbooks/httpd.yml` on `jump host` and perform the following tasks on `App Server 2`.

a. Install `httpd` and `php` packages (whatever default version is available in yum repo).

b. Change default document root of Apache to `/var/www/html/myroot` in default Apache config `/etc/httpd/conf/httpd.conf`. Make sure `/var/www/html/myroot` path exists (if not please create the same).

c. There is a template `~/playbooks/templates/phpinfo.php.j2` on `jump host`. Copy this template to the Apache document root you created as `phpinfo.php` file and make sure user owner and the group owner for this file is `apache` user.

d. Start and enable `httpd` service.

`Note:` Validation will try to run the playbook using command `ansible-playbook -i inventory httpd.yml`, so please make sure the playbook works this way without passing any extra arguments.

# Solution

`cd ~/playbooks`

`vi httpd.yml`

```yaml

- name: Setup httpd and php on App Server 2
  hosts: stapp02
  become: yes
  tasks:

    - name: Install latest version of httpd and php
      package:
        name:
          - httpd
          - php
        state: latest

    - name: Replace default DocumentRoot in httpd.conf
      replace:
        path: /etc/httpd/conf/httpd.conf
        regexp: DocumentRoot \"\/var\/www\/html\"
        replace: DocumentRoot "/var/www/html/myroot"

    - name: If non-existent, create the new DocumentRoot directory 
      file:
        path: /var/www/html/myroot
        state: directory
        owner: apache
        group: apache

    - name: Generate phpinfo.php
      template:
        src: /home/thor/playbooks/templates/phpinfo.php.j2
        dest: /var/www/html/myroot/phpinfo.php
        owner: apache
        group: apache

    - name: Start and enable service httpd
      service:
        name: httpd
        state: started
        enabled: yes
```

`ansible-playbook  -i inventory httpd.yml`

```bash
thor@jumphost ~/playbooks$ ansible-playbook  -i inventory httpd.yml

PLAY [Setup httpd and php on App Server 2] **********************************************************************************

TASK [Gathering Facts] ******************************************************************************************************
ok: [stapp02]

TASK [Install latest version of httpd and php] ******************************************************************************
changed: [stapp02]

TASK [Replace default DocumentRoot in httpd.conf] ***************************************************************************
changed: [stapp02]

TASK [If non-existent, create the new DocumentRoot directory] ***************************************************************
changed: [stapp02]

TASK [Generate phpinfo.php] *************************************************************************************************
changed: [stapp02]

TASK [Start and enable service httpd] ***************************************************************************************
changed: [stapp02]

PLAY RECAP ******************************************************************************************************************
stapp02                    : ok=6    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```

Check the result:

`ansible -i inventory stapp02 -m shell -a "rpm -qa | grep -e httpd -e php”`

```bash
thor@jumphost ~/playbooks$ ansible -i inventory stapp02 -m shell -a "rpm -qa | grep -e httpd -e php"
stapp02 | CHANGED | rc=0 >>
php-common-8.0.30-1.el9.x86_64
httpd-filesystem-2.4.57-8.el9.noarch
httpd-tools-2.4.57-8.el9.x86_64
php-cli-8.0.30-1.el9.x86_64
php-opcache-8.0.30-1.el9.x86_64
php-pdo-8.0.30-1.el9.x86_64
php-fpm-8.0.30-1.el9.x86_64
php-xml-8.0.30-1.el9.x86_64
centos-logos-httpd-90.8-1.el9.noarch
php-mbstring-8.0.30-1.el9.x86_64
httpd-core-2.4.57-8.el9.x86_64
httpd-2.4.57-8.el9.x86_64
php-8.0.30-1.el9.x86_64
```

`ansible -i inventory stapp02 -m shell -a "rpm -qa | grep -e httpd -e php”`

```bash
thor@jumphost ~/playbooks$ ansible -i inventory stapp02 -m shell -a "sudo systemctl status httpd"
stapp02 | CHANGED | rc=0 >>
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
    Drop-In: /usr/lib/systemd/system/httpd.service.d
             └─php-fpm.conf
     Active: active (running) since Tue 2024-06-25 09:19:46 UTC; 1min 55s ago
       Docs: man:httpd.service(8)
   Main PID: 4439 (httpd)
     Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: 0; Bytes served/sec:   0 B/sec"
      Tasks: 177 (limit: 1340692)
     Memory: 17.2M
```
