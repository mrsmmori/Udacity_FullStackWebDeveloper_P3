Udacity_FullStackWebDeveloper_P3 / Linux Server Configuration
====

# Overview
This is a mock project for Udacity Nanodegree / Full Stack Web Developer P3.
There are 3 points to achieve.

1. Perform a baseline installation of a Linux server 
2. Secure your server from a number of attack vectors, install and configure a database server
3. Deploy one of your existing web applications onto it.

Reference: <a href="https://review.udacity.com/#!/rubrics/2007/view">Linux Server Configuration: Rubrics</a>


# How to login Ubuntu and open HTML

Use "id_rsa" file as a private key.
ssh grader@159.89.196.120 -p 2200

http://159.89.196.120

# Summary of software installed

1. python 3.6.3
```
mrsmmori@ubuntu-s-1vcpu-1gb-sgp1-01:~$ pip freeze
Click==7.0
Flask==1.0.2
Flask-SQLAlchemy==2.3.2
httplib2==0.12.1
itsdangerous==1.1.0
Jinja2==2.10
MarkupSafe==1.1.1
mod-wsgi==4.6.5
oauth2client==4.1.3
psycopg2==2.7.7
psycopg2-binary==2.7.7
pyasn1==0.4.5
pyasn1-modules==0.2.4
rsa==4.0
six==1.12.0
SQLAlchemy==1.3.1
Werkzeug==0.15.1
```

2. Debian packages
```
gcc 
make 
libssl-dev 
libbz2-dev 
libreadline-dev 
libsqlite3-dev 
zlib1g-dev
postgresql 
postgresql-contrib
apache2
apache2-dev
postgresql 
postgresql-contrib
```



# Summary of configurations made

## Ubuntu security configurations

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

firewall configuration

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

public key setup

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

## Postgresql configurations

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

## Apache2 configurations

```
root@ubuntu-s-1vcpu-1gb-sgp1-01:/etc/apache2/sites-available# cat 000-default.conf
<VirtualHost *:80>
    LoadModule wsgi_module "/home/mrsmmori/.pyenv/versions/3.6.3/lib/python3.6/site-packages/mod_wsgi/server/mod_wsgi-py36.cpython-36m-x86_64-linux-gnu.so"
    ServerName 159.89.196.120
    WSGIProcessGroup myapp
    WSGIDaemonProcess myapp user=mrsmmori group=mrsmmori python-path=/home/mrsmmori/.pyenv/versions/3.6.3/lib/python3.6/site-packages python-home=/home/mrsmmori/.pyenv/versions/3.6.3
    WSGIScriptAlias / /home/mrsmmori/myapp/test.wsgi.py

    <Directory /home/mrsmmori/myapp>
        <Files test.wsgi.py>
                Require all granted
        </Files>
    </Directory>
</VirtualHost>
root@ubuntu-s-1vcpu-1gb-sgp1-01:/etc/apache2/sites-available#
```

## mod_wsgi configurations

```
mrsmmori@ubuntu-s-1vcpu-1gb-sgp1-01:~/myapp$ cat test.wsgi.py
import sys, site

site.addsitedir('/home/mrsmmori/.pyenv/versions/2.7.16/lib/python2.7/site-packages')

sys.path.insert(0, '/home/mrsmmori/myapp')

from app import app as application
mrsmmori@ubuntu-s-1vcpu-1gb-sgp1-01:~/myapp$
```

# List of external resources

- Github repos
- StackOverflow posts
- Udacity Full stack nanodegree course materials.


# Author

[mrsmmori](https://github.com/mrsmmori)

