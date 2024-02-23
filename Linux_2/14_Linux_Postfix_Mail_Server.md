# Instructions

`xFusionCorp Industries` has planned to set up a common email server in `Stork DC`. After several meetings and recommendations they have decided to use `postfix` as their `mail transfer agent` and `dovecot` as an `IMAP/POP3` server. We would like you to perform the following steps:

1. Install and configure `postfix` on `Stork DC` mail server.

2. Create an email account `siva@stratos.xfusioncorp.com` identified by `B4zNgHA7Ya`.

3. Set its mail directory to `/home/siva/Maildir`.

4. Install and configure `dovecot` on the same server.

# Solution

`ssh groot@stmail01`

`sudo su`

`rpm -qa | grep postfix` 

`rpm -qa | grep devecot`

`yum install postfix -y`

`vi /etc/postfix/main.cf`

Change these lines:

```
myhostname = [stmail01.stratos.xfusioncorp.com](http://stmail01.stratos.xfusioncorp.com/)

mydomain = [stratos.xfusioncorp.com](http://stratos.xfusioncorp.com/)

myorigin = $mydomain

inet_interfaces = all

#inet_interfaces = localhost

#mydestination = $myhostname, localhost.$mydomain, localhost
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain

mynetworks = 172.16.238.0/24, 127.0.0.0/8

home_mailbox = Maildir/
```

`systemctl start postfix`

`systemctl status postfix`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/6c26aaf9-681e-4a25-a893-65bfcc1e4e41)

`useradd siva`

`passwd siva` (`B4zNgHA7Ya`)

`cat /etc/passwd | grep siva`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/3c7a79c8-8ff5-44dc-9d90-cde636855e7d)

Add these commands:

```
telnet stmail01 25

EHLO localhost

mail from:siva@stratos.xfusioncorp.com

rcpt to:siva@stratos.xfusioncorp.com

DATA

Test mail

.
```

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/3f032d50-07dd-4051-9b6a-67786d15b5ed)

`su - siva`

`cd Maildir`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/d0408f78-1a48-41fe-94d7-86eb03c7f4a4)

Check `new`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/163eeaba-2216-4ba8-ae09-4647cedf656f)

Go back to stmail01 the and install dovecot

`yum install dovecot -y`

`vi /etc/dovecot/dovecot.conf`

Update this line:

```
protocols = imap pop3 lmtp submissio
```

`vi /etc/dovecot/conf.d/10-mail.conf`

Change this line:

```
mail_location = maildir:~/Maildir
```

`vi /etc/dovecot/conf.d/10-auth.conf`

Change these lines:

```
disable_plaintext_auth = yes
auth_mechanisms = plain login
```

`vi /etc/dovecot/conf.d/10-master.conf`

Change these lines:

```
    user = postfix
    group = postfix
```

`systemctl start dovecot`

`systemctl status dovecot`

`telnet stmail01 110`

And follow this screenshot:

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/86ff76e2-db8b-4a2d-a83f-8951f7a0a7c5)



