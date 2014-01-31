Introduction
-------------

Creating table is one of the most important activities on the database creation.First check if the database is alive or not.

::

  santosh@santosh-PI945GCM ~ $ mysqladmin ping -u root -p
  Enter password: 
  mysqld is alive


Logging into the session.
---------------------------

::

  santosh@santosh-PI945GCM ~ $ mysql -u root -p
  Enter password: 
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 40
  Server version: 5.5.31-0ubuntu0.13.04.1 (Ubuntu)
  Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.
  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.
  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  mysql>

Check the various databases
----------------------------

::

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
  5 rows in set (0.02 sec)


Use a databases
-----------------

::

  mysql> use test;
  Database changed


Create the tables
------------------

::

  mysql> CREATE TABLE pet (name VARCHAR(20), owner VARCHAR(20),species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
  Query OK, 0 rows affected (0.09 sec)



  mysql> CREATE TABLE if not exists pet (name VARCHAR(20), owner VARCHAR(20),species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);
  Query OK, 0 rows affected, 1 warning (0.00 sec)


Describe/desc/Explain
----------------------

::

  mysql> explain pet;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.01 sec)



  mysql> desc pet;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.00 sec)



  mysql> describe pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.00 sec)


  mysql> show columns from pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.00 sec)



How to create temporary tables:
---------------------------------

::

  mysql> create temporary table pet_temp select * from pet;
  Query OK, 0 rows affected (0.09 sec)
  Records: 0  Duplicates: 0  Warnings: 0



  mysql> describe pet_temp;
  
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.01 sec)



  mysql> show tables;

  +----------------+
  | Tables_in_test |
  +----------------+
  | pet            |
  +----------------+
  1 row in set (0.00 sec)


Altering Existing Tables
--------------------------

Changing tables is just as easy as creating them, you just have to know what you want to change. The column name is very different from the table's name. Changing the column's type is different from changing a column's name. Check out the following examples to see how to alter a column's name, type, and the table's name.

Changing a column name
-----------------------

Let change the owner to Powner.

::

  mysql> desc pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.01 sec)

  mysql> alter table pet change owner Powner varchar(20);
  Query OK, 0 rows affected (0.29 sec)
  Records: 0  Duplicates: 0  Warnings: 0


  mysql> describe pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | Powner  | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.01 sec)

Changing a column type
-------------------------

::

  mysql> alter table pet change Powner owner varchar(30);
  Query OK, 0 rows affected (0.22 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> desc pet;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(30) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.00 sec)


  mysql> show tables;
  
  +----------------+
  | Tables_in_test |
  +----------------+
  | pet_table      |
  +----------------+
  1 row in set (0.00 sec)


  mysql> desc pet_table;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(30) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.00 sec)


Deleting/Adding columns and tables
------------------------------------

To delete an existing table, type

::

  mysql> show tables;

  +----------------+
  | Tables_in_test |
  +----------------+
  | pet_table      |
  +----------------+
  1 row in set (0.00 sec)


  mysql> drop table pet_table;
  Query OK, 0 rows affected (0.04 sec)


  mysql> show tables;
  Empty set (0.00 sec)


This will delete the entire table and all the data inside the table. Use caution when executing this command. Remember there are no warnings from the monitor. Afteryou drop something, the only way to get it back is through a backup log.

Dropping a column name
-----------------------

::
 
  mysql> desc pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.01 sec)

  mysql> 

  mysql> alter table pet drop death;
  Query OK, 0 rows affected (0.21 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> desc pet;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  5 rows in set (0.00 sec)


Adding a column name:
-----------------------

::

  mysql> desc pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  5 rows in set (0.00 sec)


  mysql> alter table pet add death date;
  Query OK, 0 rows affected (0.21 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> desc pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.01 sec)


Creating of table using file redirect
---------------------------------------

::

  mysql> select database();
  
  +------------+
  | database() |
  +------------+
  | test       |
  +------------+
  1 row in set (0.00 sec)

  mysql> show tables;
  Empty set (0.00 sec)

  santosh@santosh-PI945GCM ~ $ mysql -u root -p < /tmp/pet.txt
  Enter password: 

  mysql> show tables;
  
  +----------------+
  | Tables_in_test |
  +----------------+
  | pet            |
  +----------------+
  1 row in set (0.00 sec)

  mysql> desc pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+

  6 rows in set (0.01 sec)

Entering data into the tables
--------------------------------

There are various ways of inserting data into tables,

* LOAD DATA INFILE
* INSERT
* IMPORT WITH BATCHING
* MYSQLIMPORT
* OUTFILE (misc)


LOAD DATA INFILE
------------------

Lets a take a file, '/tmp/table1_pet.txt' .

::

  santosh@santosh-PI945GCM /tmp $ cat table1_pet.txt
  Fluffy,Harold,cat,f,1992-02-04,\N,
  Claws,Gwen,cat,m,1994-03-17,\N,
  Bluffy,Harold,dog,f,1989-05-13,\N,
  Fang,Benny,dog,m,1990-08-27,\N,
  Bowser,Diana,dog,m,1979-08-31,1995-7-29,
  Chirpy,Gwen,bird,f,1998-09-11,\N,
  Whistler,Gwen,bird,\N,1997-12-09,\N,
  Slim,Benny,snake,m,1996-04-29,\N,
  
  mysql> help 'load data';
  mysql> LOAD DATA INFILE '/tmp/table1_pet.txt'
      -> INTO TABLE pet
      -> FIELDS terminated by ',';
  Query OK, 8 rows affected (0.07 sec)
  Records: 8  Deleted: 0  Skipped: 0  Warnings: 0


  mysql> select * from pet;
  
  +----------+--------+---------+------+------------+------------+
  | name     | owner  | species | sex  | birth      | death      |
  +----------+--------+---------+------+------------+------------+
  | Fluffy   | Harold | cat     | f    | 1992-02-04 | NULL       |
  | Claws    | Gwen   | cat     | m    | 1994-03-17 | NULL       |
  | Bluffy   | Harold | dog     | f    | 1989-05-13 | NULL       |
  | Fang     | Benny  | dog     | m    | 1990-08-27 | NULL       |
  | Bowser   | Diana  | dog     | m    | 1979-08-31 | 1995-07-29 |
  | Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
  | Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
  | Slim     | Benny  | snake   | m    | 1996-04-29 | NULL       |
  +----------+--------+---------+------+------------+------------+

  8 rows in set (0.01 sec)


INSERT
--------

one other way of inserting the values into the table is using the insert option.

::

  mysql> insert into  pet values ('Fluffy','Harold','cat','f','1992-02-04',NULL);
  Query OK, 1 row affected (0.04 sec)

  mysql> select * from pet;

  +--------+--------+---------+------+------------+-------+
  | name   | owner  | species | sex  | birth      | death |
  +--------+--------+---------+------+------------+-------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL  |
  +--------+--------+---------+------+------------+-------+
  1 row in set (0.00 sec)

  mysql> insert into  pet (name,owner,species,sex,birth,death) values ('Fluffy','Harold','cat','f','1992-02-04',NULL);
  Query OK, 1 row affected (0.05 sec)

  mysql> select * from pet;

  +--------+--------+---------+------+------------+-------+
  | name   | owner  | species | sex  | birth      | death |
  +--------+--------+---------+------+------------+-------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL  |
  +--------+--------+---------+------+------------+-------+
  1 row in set (0.00 sec)


IMPORT WITH BATCHING
----------------------

First create a table with the followoing entries.

::

  santosh@santosh-PI945GCM /var/www/html/table $ cat /tmp/insert_table1_pet.txt 
  insert into pet values ('Fluffy','Harold','cat','f','1992-02-04',NULL);
  insert into pet values ('Claws','Gwen','cat','m','1994-03-17',NULL);
  insert into pet values ('Bluffy','Harold','dog','f','1989-05-13',NULL);
  insert into pet values ('Fang','Benny','dog','m','1990-08-27',NULL);
  insert into pet values ('Bowser','Diana','dog','m','1979-08-31','1995-7-29');
  insert into pet values ('Chirpy','Gwen','bird','f','1998-09-11',NULL);
  insert into pet values ('Whistler','Gwen','bird','NULL','1997-12-09',NULL);
  insert into pet values ('Slim','Benny','snake','m','1996-04-29',NULL);
  
Now lets insert values into 'pet' in database 'test'.

::

  santosh@santosh-PI945GCM /var/www/html/table $ mysql -u root -p test < /tmp/insert_table1_pet.txt 
  Enter password: 


  mysql> select * from pet;

  +----------+--------+---------+------+------------+------------+
  | name     | owner  | species | sex  | birth      | death      |
  +----------+--------+---------+------+------------+------------+
  | Fluffy   | Harold | cat     | f    | 1992-02-04 | NULL       |
  | Claws    | Gwen   | cat     | m    | 1994-03-17 | NULL       |
  | Bluffy   | Harold | dog     | f    | 1989-05-13 | NULL       |
  | Fang     | Benny  | dog     | m    | 1990-08-27 | NULL       |
  | Bowser   | Diana  | dog     | m    | 1979-08-31 | 1995-07-29 |
  | Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
  | Whistler | Gwen   | bird    | N    | 1997-12-09 | NULL       |
  | Slim     | Benny  | snake   | m    | 1996-04-29 | NULL       |
  +----------+--------+---------+------+------------+------------+

  8 rows in set (0.00 sec)



MYSQLIMPORT
-------------

First create a table /tmp/test.txt with followoing entries. Our goal is to populate the entries of the table test.

::

  santosh-PI945GCM tmp # cat test.txt
  1 
  2 
  3 
  4   
  santosh-PI945GCM tmp # 


  santosh-PI945GCM tmp # mysqlimport -u root -p  --local test test.txt
  Enter password: 
  test.test: Records: 4  Deleted: 0  Skipped: 0  Warnings: 0


  mysql> desc test;
  
  +-------+---------+------+-----+---------+-------+
  | Field | Type    | Null | Key | Default | Extra |
  +-------+---------+------+-----+---------+-------+
  | id    | int(11) | YES  |     | NULL    |       |
  +-------+---------+------+-----+---------+-------+
  1 row in set (0.03 sec)
  
  mysql> select * from test;
  +------+
  | id   |
  +------+
  |    1 |
  |    2 |
  |    3 |
  |    4 |
  +------+
  4 rows in set (0.00 sec)


OUTFILE
---------

Sometimes we want the output of the table in form of a file.

::

  mysql> select * into outfile '/tmp/output_pet.txt' from pet;
  Query OK, 8 rows affected (0.01 sec)

The output of output_pet.txt is tab delimited fields.

::

  santosh-PI945GCM tmp # cat output_pet.txt 
  Fluffy  Harold  cat f 1992-02-04  \N
  Claws Gwen  cat m 1994-03-17  \N
  Bluffy  Harold  dog f 1989-05-13  \N
  Fang  Benny dog m 1990-08-27  \N
  Bowser  Diana dog m 1979-08-31  1995-07-29
  Chirpy  Gwen  bird  f 1998-09-11  \N
  Whistler  Gwen  bird  \N  1997-12-09  \N
  Slim  Benny snake m 1996-04-29  \N

