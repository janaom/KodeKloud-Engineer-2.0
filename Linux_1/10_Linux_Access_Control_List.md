# Instructions

The `Nautilus` security team performed an audit on all servers present in `Stratos DC`. During the audit some critical data/files were identified which were 
having the wrong permissions as per security standards. Once the report was shared with the production support team, they started fixing the issues one by one. It has been identified that one of the files named `/etc/hosts` on `Nautilus App 1` server has wrong permissions, so that needs to be fixed and the correct ACLs needs to be set.

1. The user owner and group owner of the  file should be `root` user.

2. `Others` must have `read only` permissions on the file.

3. User `yousuf` must not have any permission on the file.

4. User `eric` should have `read only` permission on the file.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

Check ACL of the file: `getfacl /etc/hosts`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/b61e3e4a-d8cc-46de-bdba-11582742f8b9)

- `# owner: root`: This line indicates that the owner of the file is the root user.
- `# group: root`: This line indicates that the group owner of the file is the root group.
- `user::rw-`: This line represents the permissions for the file owner (root). Here's what each character signifies:
    - `user`: Refers to the file owner.
    - `::`: Separates the user/group specification from the permissions.
    - `rw-`: Indicates that the owner has read (`r`) and write (`w`) permissions, but no execute (``) permission.
- `group::r--`: This line represents the permissions for the group owner (root). Here's what each character signifies:
    - `group`: Refers to the group owner.
    - `::`: Separates the user/group specification from the permissions.
    - `r--`: Indicates that the group has read (`r`) permission, but no write (``) or execute (``) permission.
- `other::r--`: This line represents the permissions for other users (users not in the owner group). Here's what each character signifies:
    - `other`: Refers to other users.
    - `::`: Separates the user/group specification from the permissions.
    - `r--`: Indicates that other users have read (`r`) permission, but no write (``) or execute (``) permission.

To ensure that others have read-only permissions on a file, you can use the `chmod` command with the appropriate permissions: `sudo chmod o=r /etc/hosts`

To modify the access control list (ACL) and set permissions for the user "yousuf" to have no access on a file, you can use the following command: `sudo setfacl -m u:yousuf:--- /etc/hosts`

In this command, `-m` specifies the modification of the access control list (ACL), `u:eric` indicates the user "eric" whose permissions should be modified, and `r` grants read-only permission: `sudo setfacl -m u:eric:r /etc/hosts`

Check the result 

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/9c8ae850-958e-489e-a967-2869faffda83)

