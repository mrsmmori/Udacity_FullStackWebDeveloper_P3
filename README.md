Udacity_FullStackWebDeveloper_P3 / Linux Server Configuration
====

# Overview
You will take a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.

<a href="https://review.udacity.com/#!/rubrics/2007/view">Linux Server Configuration: Rubrics</a>




# Usage
http://159.89.196.120

Use "id_rsa" as private key.

ssh grader@159.89.196.120 -p 2200



# Summary of software installed

1. python 3.6
2. python moduels
	flask, flask_sqlalchemy, oauth2client, uwsgi
3. postgresql, postgresql-contrib
4. apache2, apache2-dev



# Summary of configurations made

## Ubuntu Security Settings

sshd_conf
```bash
root@ubuntu-s-1vcpu-1gb-sgp1-01:/etc/ssh# diff sshd_config sshd_config.original
13c13
< Port 2200
---
> #Port 22
32c32
< PermitRootLogin no
---
> PermitRootLogin yes
37c37
< PubkeyAuthentication yes
---
> #PubkeyAuthentication yes
56c56
< PasswordAuthentication no
---
> PasswordAuthentication yes
```

firewall
```bash
root@ubuntu-s-1vcpu-1gb-sgp1-01:/etc/ssh# ufw status
Status: active

To                         Action      From
--                         ------      ----
123                        ALLOW       Anywhere
80                         ALLOW       Anywhere
2200                       ALLOW       Anywhere
123 (v6)                   ALLOW       Anywhere (v6)
2200 (v6)                  ALLOW       Anywhere (v6)
80 (v6)                    ALLOW       Anywhere (v6)

```

public key
```
(2.7.12) $ ssh-keygen
(2.7.12) $ cd /c/Users/mrsmm/.ssh/
(2.7.12) $ ls
id_rsa  id_rsa.pub  known_hosts

grader@ubuntu-s-1vcpu-1gb-sgp1-01:~$ mkdir .ssh
grader@ubuntu-s-1vcpu-1gb-sgp1-01:~$ vi .ssh/authorized_keys
grader@ubuntu-s-1vcpu-1gb-sgp1-01:~$ chmod 700 .ssh
grader@ubuntu-s-1vcpu-1gb-sgp1-01:~$ chmod 600 .ssh/authorized_keys
```

## Postgresql Settings

```
postgres=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
 mrsmmori  | Create DB                                                  | {}
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}

mrsmmori@ubuntu-s-1vcpu-1gb-sgp1-01:~$ psql catalog
psql (10.6 (Ubuntu 10.6-0ubuntu0.18.04.1))
Type "help" for help.

catalog=> \l
                              List of databases
   Name    |  Owner   | Encoding | Collate |  Ctype  |   Access privileges
-----------+----------+----------+---------+---------+-----------------------
 catalog   | mrsmmori | UTF8     | C.UTF-8 | C.UTF-8 |
 postgres  | postgres | UTF8     | C.UTF-8 | C.UTF-8 |
 template0 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres
 template1 | postgres | UTF8     | C.UTF-8 | C.UTF-8 | =c/postgres          +
           |          |          |         |         | postgres=CTc/postgres
(4 rows)

```





# Author

[mrsmmori](https://github.com/mrsmmori)







Prepare to deploy your project.
9. Configure the local timezone to UTC.
10. Install and configure Apache to serve a Python mod_wsgi application.


install the Python 3 mod_wsgi package on your server: sudo apt-get install libapache2-mod-wsgi-py3.

Create a new database user named catalog that has limited permissions to your catalog application database.

