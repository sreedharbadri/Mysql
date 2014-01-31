Introduction
-------------------------

A key on a database table provides a means to rapidly locate specific information. Although a key need not mean anything to the human user of the database, keys are a vital part of the database architecture, and can significantly influence performance.

How keys work
----------------------------

A key exists like an extra table in the database, although it belonging to its parent table. It takes up physical space on the hard disk (or other storage areas) ofthe database. It can be as big as the main table and,theoretically, even bigger.


You define your key to relate to one or a number of columns in a specific table. Because the data in a key is totally derived from the table, you can drop and re-create a key without any loss of data. As you will see, there may be good reasons for doing this.

Benefits of using a key
-----------------------------

Proper use of keys can significantly improve database performance. For example take book index , consider how few pages it takes in the index of a book to give you a fast way of searching for the important themes.Compare that to how long it would take if you were scanning through the volume page-by-page.


Keys in modern databases are designed to minimize the amount of disk accessing needed when reading from them. Even the method of scanning through the key and determining whether the data matches what you're looking for comprises sophisticated matching algorithms.


For help: 
---------------------

::

   mysql> help 'alter table';

we can set index on tables also using the  CREATE command.

::

  CREATE INDEX index_name ON table_name (column_name[,...]);
  CREATE UNIQUE INDEX [index_name] ON table_name (column_name[,...]);
  CREATE PRIMARY KEY ON table_name (column_name,...);

::

   mysql> help 'create index';


Single column keys
-----------------------

Lets try setting up a key on one of the tables.

Description of the table pet before creating index:

::

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


Creating a index on the column name.
-------------------------------------

Let try to create a index on the column name using the alter table.

::


  mysql> alter table pet add index name(name);
  Query OK, 0 rows affected (0.22 sec)
  Records: 0  Duplicates: 0  Warnings: 0

The MUL in the key field shows its a non-unique key field.

::

  mysql> explain pet;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  | MUL | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  6 rows in set (0.00 sec)

To see keys on the table
--------------------------

::

  mysql> show keys from pet;

  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | pet   |          1 | name     |            1 | name        | A         |           8 |     NULL | NULL   | YES  | BTREE      |         |               |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  1 row in set (0.00 sec)

  mysql> show index from pet;
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | pet   |          1 | name     |            1 | name        | A         |           8 |     NULL | NULL   | YES  | BTREE      |         |               |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  1 row in set (0.00 sec)



Multiple column keys
---------------------

You can create a multiple-column key, also called a composite index, on more than one column in the same  table.

::

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

  mysql> alter table pet add index comp_key(name,owner,species);
  Query OK, 0 rows affected (0.19 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> show keys from pet;
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | pet   |          1 | comp_key |            1 | name        | A         |           8 |     NULL | NULL   | YES  | BTREE      |         |               |
  | pet   |          1 | comp_key |            2 | owner       | A         |           8 |     NULL | NULL   | YES  | BTREE      |         |               |
  | pet   |          1 | comp_key |            3 | species     | A         |           8 |     NULL | NULL   | YES  | BTREE      |         |               |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  3 rows in set (0.00 sec)

Multiple-column keys present a greater database overhead than single-column keys. Bear this in mind when deciding whether to use a multiple- or single-column key.

Partial keys
---------------

When creating a key on a CHAR or VARCHAR type column, it is possible to index the first few characters of the column. You reference the first part, or prefix, of a column by appending (length) to the name of the column.

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
  6 rows in set (0.00 sec)

  mysql> alter table pet add index name(name(5));
  Query OK, 0 rows affected (0.14 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> describe pet;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  | MUL | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  6 rows in set (0.00 sec)

  mysql> show keys from pet;
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | pet   |          1 | name     |            1 | name        | A         |           8 |        5 | NULL   | YES  | BTREE      |         |               |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  1 row in set (0.00 sec)
  

Composite partial keys
--------------------------

one can create partial keys on multiple columns.

::

  mysql> alter table pet add index comp_key(name(5),owner(5),species(5));
  Query OK, 0 rows affected (0.15 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> show keys from pet;
  
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  | pet   |          1 | comp_key |            1 | name        | A         |           8 |        5 | NULL   | YES  | BTREE      |         |               |
  | pet   |          1 | comp_key |            2 | owner       | A         |           8 |        5 | NULL   | YES  | BTREE      |         |               |
  | pet   |          1 | comp_key |            3 | species     | A         |           8 |        5 | NULL   | YES  | BTREE      |         |               |
  +-------+------------+----------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
  3 rows in set (0.00 sec)

Unique keys
-------------------

By definition, a unique index allows only unique values in the column. When you construct a WHERE clause to access one specific row, the unique key will take you toone—and only one—matching row.


Unique keys are not only used for performance, but also for ensuring data integrity. MySQL will not allow the creation of a second row with duplicate key data.


If you try to insert duplicate data into a table where a column is unique, MySQL will give you an error.The same will happen if you try to update an existing row tomake a unique column the same as another existing row.


Additionally, if you try to alter a table and add a unique key to a column where data in that column is  already non-unique, it will generate an error.


Lets go via an example which illustrates the same.

::

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


  mysql> alter table pet add unique key name(name);
  Query OK, 0 rows affected (0.16 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> describe pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  | UNI | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  6 rows in set (0.00 sec)

  mysql> insert into pet values ('puppy','santosh','dog','m','2013-07-14',NULL);
  Query OK, 1 row affected (0.05 sec)

  mysql> select * from pet;

  +-------+---------+---------+------+------------+-------+
  | name  | owner   | species | sex  | birth      | death |
  +-------+---------+---------+------+------------+-------+
  | puppy | santosh | dog     | m    | 2013-07-14 | NULL  |
  +-------+---------+---------+------+------------+-------+
  1 row in set (0.00 sec)
  

since name is a unique key , we cannot add another value with same name.

::

  mysql> insert into pet values ('puppy','santosh','dog','m','2013-07-14',NULL);
  ERROR 1062 (23000): Duplicate entry 'puppy' for key 'name'
  mysql> 

A table can have more than one unique keys.

::

  mysql> alter table pet add unique key owner(owner);
  Query OK, 0 rows affected (0.19 sec)
  Records: 0  Duplicates: 0  Warnings: 0


  mysql> desc pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  | UNI | NULL    |       |
  | owner   | varchar(20) | YES  | UNI | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  6 rows in set (0.00 sec)


Primary keys
--------------------------------

A primary key is similar in principle to a unique key; its data must be unique, but the primary key of a table has a more privileged status.


A primary key is generally used as a structural link in the database, defining relationships between different tables. Wherever you want to join from one table to another table, you would like to have that table's primary key. In contrast, keys that are merely unique can be added purely for performance reasons.


MySQL requires you to specify NOT NULL when creating a table with a given column specified as PRIMARY KEY, even if it may allow a unique key to have null values.


The choice of a primary key is very important in the design of a database; a primary key is the fundamental piece of data that facilitates the joining of tables and the whole concept of a relational database. This is why you must be careful to base your primary key on information that will always be unique.


Multi-Column primary key
----------------------------

It is possible to create a primary key as a multiple-column key.You cannot do this in your CREATE TABLE statement, but must use the ALTER TABLE syntax, as shown in the following:

::

  mysql> alter table pet add primary key (name,owner);
  Query OK, 1 row affected (0.22 sec)
  Records: 1  Duplicates: 0  Warnings: 0

  mysql> desc pet;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | NO   | PRI |         |       |
  | owner   | varchar(20) | NO   | PRI |         |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  6 rows in set (0.00 sec)

Partial Primary keys:
----------------------

::

  mysql> alter table pet add primary key(name(5),owner(5));
  Query OK, 0 rows affected (0.26 sec)
  Records: 0  Duplicates: 0  Warnings: 0

  mysql> desc pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | NO   | PRI |         |       |
  | owner   | varchar(20) | NO   | PRI |         |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  6  rows in set (0.00 sec)


Examples on Primary key:
-------------------------

Lets create two table p_t and s_t.

::

  mysql> create table p_t (eid int auto_increment unique,name varchar(10)) engine=Innodb; 
  Query OK, 0 rows affected (0.10 sec)
  
  mysql> desc p_t;
  +-------+-------------+------+-----+---------+----------------+
  | Field | Type        | Null | Key | Default | Extra          |
  +-------+-------------+------+-----+---------+----------------+
  | eid   | int(11)     | NO   | PRI | NULL    | auto_increment |
  | name  | varchar(10) | YES  |     | NULL    |                |
  +-------+-------------+------+-----+---------+----------------+
  2 rows in set (0.00 sec)
  
  mysql> create table s_t ( id int auto_increment unique,empid int ,shift enum('morning','evening'), FOREIGN KEY (empid) REFERENCES p_t (eid) ON DELETE CASCADE ON U  PDATE CASCADE) engine=Innodb;
  Query OK, 0 rows affected (0.11 sec)
  
  mysql> desc s_t;
  +-------+---------------------------+------+-----+---------+----------------+
  | Field | Type                      | Null | Key | Default | Extra          |
  +-------+---------------------------+------+-----+---------+----------------+
  | id    | int(11)                   | NO   | PRI | NULL    | auto_increment |
  | empid | int(11)                   | YES  | MUL | NULL    |                |
  | shift | enum('morning','evening') | YES  |     | NULL    |                |
  +-------+---------------------------+------+-----+---------+----------------+
  3 rows in set (0.00 sec)


  mysql> insert into p_t values (1,'santosh'); 
  Query OK, 1 row affected (0.04 sec)
  
  mysql>  insert into p_t values (2,'dinesh'); 
  Query OK, 1 row affected (0.05 sec)
  
  mysql> insert into p_t values (3,'hari'); 
  Query OK, 1 row affected (0.03 sec)
  
  mysql> select * from p_t;
  +-----+---------+
  | eid | name    |
  +-----+---------+
  |   1 | santosh |
  |   2 | dinesh  |
  |   3 | hari    |
  +-----+---------+
  3 rows in set (0.00 sec)
  
  mysql> insert into s_t values (1,1,'morning'); 
  Query OK, 1 row affected (0.07 sec)
  
  mysql> insert into s_t values (2,2,'evening'); 
  Query OK, 1 row affected (0.03 sec)
  
  mysql> select * from s_t;
  +----+-------+---------+
  | id | empid | shift   |
  +----+-------+---------+
  |  1 |     1 | morning |
  |  2 |     2 | evening |
  +----+-------+---------+
  2 rows in set (0.00 sec)

Lets try to insert some value which is not part of the eid range.

::

  mysql> insert into s_t values (3,4,'evening');
  ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`new`.`s_t`, CONSTRAINT `s_t_ibfk_1` FOREIGN KEY (`empid`) REFERENCES `p_t`   (`eid`) ON DELETE CASCADE ON UPDATE CASCADE)

If you notice the error says we cannot enter any other value other than the values we provided in the eid field of the p_t table.

Now lets try to enter few more values.

::

  mysql> insert into s_t values (3,3,'evening'); 
  Query OK, 1 row affected (0.05 sec)

  mysql> 
  mysql> select * from s_t;
  +----+-------+---------+
  | id | empid | shift   |
  +----+-------+---------+
  |  1 |     1 | morning |
  |  2 |     2 | evening |
  |  3 |     3 | evening |
  +----+-------+---------+
  3 rows in set (0.00 sec)
  
Now lets see how the ON UPDATE CASCADE works.

Below is the values we have in our table p_t and s_t.

::

  mysql> select * from s_t;
  +----+-------+---------+
  | id | empid | shift   |
  +----+-------+---------+
  |  1 |     1 | morning |
  |  2 |     2 | evening |
  |  3 |     3 | evening |
  +----+-------+---------+
  3 rows in set (0.00 sec)
  
  mysql> select * from p_t;
  +-----+---------+
  | eid | name    |
  +-----+---------+
  |   1 | santosh |
  |   2 | dinesh  |
  |   3 | hari    |
  +-----+---------+
  3 rows in set (0.00 sec)

Hari is not ok with his eid 3 , so lets try to modify it to 33 . According to constraints if we modify eid to 33 , it should reflect in other tables.

::

  mysql> update p_t set eid=33 where name='hari';
  Query OK, 1 row affected (0.04 sec)
  Rows matched: 1  Changed: 1  Warnings: 0
  
  mysql> select * from p_t;
  +-----+---------+
  | eid | name    |
  +-----+---------+
  |   1 | santosh |
  |   2 | dinesh  |
  |  33 | hari    |
  +-----+---------+
  3 rows in set (0.00 sec)
  
  mysql> select * from s_t;
  +----+-------+---------+
  | id | empid | shift   |
  +----+-------+---------+
  |  1 |     1 | morning |
  |  2 |     2 | evening |
  |  3 |    33 | evening |
  +----+-------+---------+
  3 rows in set (0.00 sec)

  
Now lets see how ON DELETE CASCADE works.

Hari wants to leave the organisation.

::

  mysql> delete from p_t where eid='33';
  Query OK, 1 row affected (0.04 sec)
  
  mysql> select * from p_t;
  +-----+---------+
  | eid | name    |
  +-----+---------+
  |   1 | santosh |
  |   2 | dinesh  |
  +-----+---------+
  2 rows in set (0.00 sec)
  
  mysql> select * from s_t;
  +----+-------+---------+
  | id | empid | shift   |
  +----+-------+---------+
  |  1 |     1 | morning |
  |  2 |     2 | evening |
  +----+-------+---------+
  2 rows in set (0.00 sec)

If you noted in the example above , we have used constraints. If we don't use the CONSTRAINTS key words mysql creates it by default.

::

  mysql> create table s_t ( id int auto_increment unique,empid int ,shift enum('morning','evening'), constraint fk_new FOREIGN KEY (empid) REFERENCES p_t (eid) ON D  ELETE CASCADE ON UPDATE CASCADE) engine=Innodb;
  Query OK, 0 rows affected (0.08 sec)
  
  mysql> show create table s_t;
  | Table | Create Table |
  ----------------------------------------------------------------------------------------------------------
  | s_t   | CREATE TABLE `s_t` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `empid` int(11) DEFAULT NULL,
    `shift` enum('morning','evening') DEFAULT NULL,
    UNIQUE KEY `id` (`id`),
    KEY `fk_new` (`empid`),
    CONSTRAINT `fk_new` FOREIGN KEY (`empid`) REFERENCES `p_t` (`eid`) ON DELETE CASCADE ON UPDATE CASCADE
  ) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
  -----------------------------------------------------------------------------------------------------------
  1 row in set (0.00 sec)

That said if you dont add the constraint it will be created automatically.

::

  mysql> show create table s_t;
  | Table | Create Table |
  ----------------------------------------------------------------------------------------------------------
  | s_t   | CREATE TABLE `s_t` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `empid` int(11) DEFAULT NULL,
    `shift` enum('morning','evening') DEFAULT NULL,
    UNIQUE KEY `id` (`id`),
    KEY `empid` (`empid`),
  
    CONSTRAINT `s_t_ibfk_1` FOREIGN KEY (`empid`) REFERENCES `p_t` (`eid`) ON DELETE CASCADE ON UPDATE CASCADE
  ) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
  ----------------------------------------------------------------------------------------------------------


In mysql for working with mysql constraints, we need to follow few :

#. The primary table should have a primary key.
#. Keys for which we are creating constaints should be of same datatypes.
#. FOREIGN KEYS support is only for the Innodb engines.
#. FOREIGN KEY column is indexed automatically, unless we specify another index for it.

When to use keys
-----------------

Whether to use a key on a column will depend on the types of queries you intend to carry out.

When to use keys:

**WHERE clauses**

If you are frequently using a column for selection criteria, a key on  this column will generally improve performance. The lower the number of rows in a  table that are likely to be returned by a SELECT...WHERE statement, the more a  key will be beneficial. In particular, when a query is expected to return a unique  result, a key should be created on the column or columns used by the WHERE clause.

**ORDER BY and GROUP BY** clauses—Sorting data is a costly exercise. Because a key automatically renders results in alphabetical order, columns that you want to ORDER  BY or GROUP BY are good candidates for keys.

**MIN() and MAX()** Keys are highly efficient at finding minimum and maximum values in a column.

**Table joins** The use of keys will always help performance where the indexed columns are being used to join tables. In general, most, if not all, columns that are at some stage used in a table join should be indexed.

Dropping a Key
----------------

If your database spends a lot of its time handling queries with read operations, and at certain times gets updated with a large number of write operations, there may be a case for using keys that are dropped just before update time and reinstated afterward.


You would drop the key with the following syntax:

Example on dropping a primary key:

::

  mysql> desc pet;

  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | NO   | PRI |         |       |
  | owner   | varchar(20) | NO   | PRI |         |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  6 rows in set (0.00 sec)

  mysql> alter table pet drop primary key ;
  Query OK, 1 row affected (0.22 sec)
  Records: 1  Duplicates: 0  Warnings: 0
  
Example on dropping a unique key:

  mysql> desc pet;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  | UNI | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  | birth   | date        | YES  |     | NULL    |       |
  | death   | date        | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  6 rows in set (0.00 sec)

  mysql> alter table pet drop key name;
  Query OK, 0 rows affected (0.13 sec)
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
  6 rows in set (0.00 sec)

