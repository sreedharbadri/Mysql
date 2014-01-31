Installation
-------------

We are going to work on mostly the Linux operating systems, so let’s try to work on installation on MySQL on the Linux operating systems.

Configuring yum
-----------------

Check if you have dump of the source CD/DVD of any Linux variant. Most of the Linux variants have MySQL bundled with the source.
Since we are mostly concentrating on the Linux variants let say we are going to work with mostly rpms.
Firstly set up yum using the redhat iso. The redhat provides MySQL as part of the Venilla .

How to set up yum:

First try mounting the redhat iso.(for example redhat-5.3.iso)

::

  # mount -o loop redhat-5.3.iso /mnt
  # mkdir /myrepos
  # cp -rv /mnt/* /myrepos


Now let’s create a yum repository.

::

  # cd /etc/yum.repos.d

create a repo file with name cdrom.repo
  
::


  # cat cdrom.repo
  [myrepos]
  name=Red Hat Enterprise Linux source dump
  baseurl=file:///cdrom
  enabled=0
  gpgcheck=1

Installation using rpm or yum 
-------------------------------

Ways of installation :

Using the rpm command.

::

  # rpm -ivh mysql-server mysql-client

Using the yum command.

::

  # yum install mysql*

Starting the services
-----------------------

Starting the services and making them permanent.

::

  # service mysqld start
  # chkconfig mysqld on

MySQL Installation Layout for Linux RPM Packages
---------------------------------------------------

+-----------------------------------------------------------------------+------------------------------------------------------------------------------+
|Directory                                                              | Contents of Directory                                                        |
+=======================================================================+==============================================================================+
|/usr/bin                                                               | Client programs and scripts                                                  |
+-----------------------------------------------------------------------+------------------------------------------------------------------------------+
|/usr/sbin                                                              | The mysqld server                                                            |
+-----------------------------------------------------------------------+------------------------------------------------------------------------------+
|/var/lib/mysql                                                         | Log files, databases                                                         |
+-----------------------------------------------------------------------+------------------------------------------------------------------------------+
|/usr/share/info                                                        | Manual in Info format                                                        |
+-----------------------------------------------------------------------+------------------------------------------------------------------------------+
|/usr/share/man                                                         | Unix manual pages                                                            |
+-----------------------------------------------------------------------+------------------------------------------------------------------------------+
|/usr/include/mysql                                                     | Include (header) files                                                       |
+-----------------------------------------------------------------------+------------------------------------------------------------------------------+
|/usr/lib/mysql                                                         | Libraries                                                                    |
+-----------------------------------------------------------------------+------------------------------------------------------------------------------+
|/usr/share/mysql                                                       | Miscellaneous support files, including error messages, character set files,  |
|                                                                       | sample configuration files, SQL for database installation                    |
+-----------------------------------------------------------------------+------------------------------------------------------------------------------+
|/usr/share/sql-bench                                                   |Benchmarksi                                                                   | 
+-----------------------------------------------------------------------+------------------------------------------------------------------------------+

Lets now try logging in and create some user id and grant some previledges to the users to start working on .

::

  santosh-PI945GCM ~ # mysql -u root -p
  Enter password: 
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 61
  Server version: 5.5.31-0ubuntu0.13.04.1 (Ubuntu)
  Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.
  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.


  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  mysql> 

Next let me understand what version of MySQL is this.

::

  mysql> select version();
  +-------------------------+
  | version()               |
  +-------------------------+
  | 5.5.31-0ubuntu0.13.04.1 |
  +-------------------------+
  1 row in set (0.00 sec)

Next let me understand the user who tried to login.

::

  mysql> select user();
  +----------------+
  | user()          |
  +----------------+
  | root@localhost |
  +----------------+
  1 row in set (0.01 sec)

Lets me now try to understand which database i am logged in.

::

  mysql> select database();
  +------------+
  | database() |
  +------------+
  | NULL       |
  +------------+
  1 row in set (0.00 sec)

Its showing a NULL, which mean i have not selected any database yet.

::

  mysql> select database();
  +------------+
  | database() |
  +------------+
  | new        |
  +------------+
  1  row in set (0.00 sec)



So , now i have selected the database new.
Now, let me create a new database test.

::

  mysql> create database test;
  Query OK, 1 row affected (0.00 sec)

  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | information_schema |
  | mysql              |
  | new                |
  | performance_schema |
  | test               |
  +--------------------+
  5 rows in set (0.00 sec)

Now let me create a user 'newuser' with all previliges for the database test.

::

  mysql> create user newuser;
  Query OK, 0 rows affected (0.00 sec)

Here the newuser is created without a password. Another way is to create a user identified by password.

::

  mysql> create user newuser identified by 'newuser';
  Query OK, 0 rows affected (0.00 sec)

Now i want to GRANT the user access to the database 'test'.

::

  mysql> grant all on test.* to 'newuser'@'localhost';
  Query OK, 0 rows affected (0.02 sec)

Please go throught the help 'grant' on mysql commond prompt to understand more about the grant previledges.

