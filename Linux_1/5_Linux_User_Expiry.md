# Instructions

A developer named kirsty has been assigned `Nautilus` project temporarily as a backup resource. As a temporary resource for this project, we need a temporary user for kirsty. It’s a good idea to create a user with an expiration date so that the user won't be able to access servers beyond that point.

Therefore, create a user named `kirsty` on the `App Server 1` in `Stratos Datacenter`. Set `expiry date` to `2021-02-17`. Make sure the user is created as per standard and is in lowercase.

# Solution

ssh into the App Server 1: `ssh tony@172.16.238.10`

Run the following command to create the user "kirsty" with a lowercase username: `sudo useradd --expiredate 2021-02-17 --create-home --shell /bin/bash kirsty`

To check if the user "kirsty" was created, you can use the `grep` command to search for the username in the `/etc/passwd` file: `grep "^kirsty:" /etc/passwd`

If the user was created successfully, you should see output similar to the following: `kirsty:x:1001:1001::/home/kirsty:/bin/bash`

To check the expiry date of the user "kirsty", you can use the `chage` command: `sudo chage -l kirsty`

This command displays the current aging information for the user "kirsty", including the expiry date. If the user has an expiry date set, you will see output similar to the following:

Last password change                                    : Nov 26, 2023
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Feb 17, 2021
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7


Or this option:

`sudo useradd --expiredate 2021-02-17 --create-home --user-group --shell /bin/bash --password $(openssl passwd -1) kirsty`

This command creates a user named "kirsty" with the specified options:

- `--expiredate 2021-02-17`: Sets the expiry date for the user account to 2021-02-17.
- `--create-home`: Creates a home directory for the user.
- `--user-group`: Creates a group with the same name as the user and assigns it as the primary group.
- `--shell /bin/bash`: Sets the user's default shell to `/bin/bash`.
- `--password $(openssl passwd -1)`: Generates an encrypted password for the user. This is an optional step, and you can set the password manually using the `passwd` command later if desired.
- `kirsty`: Specifies the username as "kirsty".

Make sure to replace `--password $(openssl passwd -1)` with `--password <encrypted_password>` if you have a specific password in mind. However, note that it is generally recommended to set the password using the `passwd` command after creating the user.

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/9c6ad238-dbb2-46b4-a0e9-d1e4e366ba3d)
