Introduction
----------------

A view is a virtual table. Views are stored queries that when invoked produce a result set.We can add SQL functions , WHERE , and JOIN statements to a view and present the data as if the data were coming from one single table.One thing to note here is view only shows the data which is up to date.Something like a latest snapshot of whatever data.

A view belongs to a database. By default, a new view is created in the default database. To create the view explicitly in a given database,specify the name as db_name.view_name when you create it:

::
  
  mysql> CREATE VIEW test.v AS SELECT * FROM t;


Help on views
--------------

We have various in build help functions in mysql.

::
  
  mysql> help 'view'
  Many help items for your request exist.
  To make a more specific request, please type 'help <item>',
  where <item> is one of the following
  topics:
   ALTER VIEW
   CREATE VIEW
   DROP VIEW

For help on CREATE,DROP or ALTER we can run help on the topics.

CREATE VIEW
------------

The syntax of creating a view is a below.

::
  
  mysql> help 'create view'
  Name: 'CREATE VIEW'
  Description:
  Syntax:
  CREATE
    [OR REPLACE]
    [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
    [DEFINER = { user | CURRENT_USER }]
    [SQL SECURITY { DEFINER | INVOKER }]
    VIEW view_name [(column_list)]
    AS select_statement
    [WITH [CASCADED | LOCAL] CHECK OPTION]

ALGORITHMS
-------------

When defining the algorithm, you have three options to choose from: MERGE, TEMPTABLE or UNDEFINED.

We now know that UNDEFINED lets MySQL choose the appropriate option, but what do the other two options mean?

MERGE is the fastest out of the two options. The view column list replaces what is in the SELECT statement, essentially merging faster than TEMPTABLE as that generates a new temporary table that is queried upon and which has no indexes. MySQL will warn you if you try to use MERGE when a TEMPTABLE should be used and will changeit to TEMPTABLE.

When will the TEMPTABLE option be used? If you use any aggregation function, DISTINCT, LIMIT, GROUP BY, HAVING, sub query or literal values (i.e. no table).


Example 1:
----------

::
  
  mysql> use new;
  Database changed
  mysql> select database();
  +------------+
  | database() |
  +------------+
  | new        |
  +------------+
  1 row in set (0.00 sec)
  
  mysql> show tables;
  Empty set (0.00 sec)
  
  mysql> create table ms(qty int, price int);
  Query OK, 0 rows affected (0.06 sec)
  
  mysql> insert into ms values(23,89);
  Query OK, 1 row affected (0.03 sec)
  
  mysql> create view msv as select qty,price,qty*price as value from ms;
  Query OK, 0 rows affected (0.02 sec)
  
  mysql> select * from msv;
  +------+-------+-------+
  | qty  | price | value |
  +------+-------+-------+
  |   23 |    89 |  2047 |
  +------+-------+-------+
  1 row in set (0.00 sec)


Example 2:
-----------

::
  
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
  8 rows in set (0.03 sec)
  
  mysql> create view pet_view as select name,owner,species from pet;
  Query OK, 0 rows affected (0.08 sec)
  
  mysql> select * from pet_view;
  +----------+--------+---------+
  | name     | owner  | species |
  +----------+--------+---------+
  | Fluffy   | Harold | cat     |
  | Claws    | Gwen   | cat     |
  | Bluffy   | Harold | dog     |
  | Fang     | Benny  | dog     |
  | Bowser   | Diana  | dog     |
  | Chirpy   | Gwen   | bird    |
  | Whistler | Gwen   | bird    |
  | Slim     | Benny  | snake   |
  +----------+--------+---------+
  8 rows in set (0.02 sec)
  
  mysql> desc pet_view;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  3 rows in set (0.01 sec)

Example 3:
-----------

::
  
  mysql> create OR replace view pet_view as select name,owner,species,sex from pet;
  Query OK, 0 rows affected (0.03 sec)
  
  mysql> desc pet_view;
  +---------+-------------+------+-----+---------+-------+
  | Field   | Type        | Null | Key | Default | Extra |
  +---------+-------------+------+-----+---------+-------+
  | name    | varchar(20) | YES  |     | NULL    |       |
  | owner   | varchar(20) | YES  |     | NULL    |       |
  | species | varchar(20) | YES  |     | NULL    |       |
  | sex     | char(1)     | YES  |     | NULL    |       |
  +---------+-------------+------+-----+---------+-------+
  4 rows in set (0.01 sec)
  
  mysql> select * from pet_view;
  +----------+--------+---------+------+
  | name     | owner  | species | sex  |
  +----------+--------+---------+------+
  | Fluffy   | Harold | cat     | f    |
  | Claws    | Gwen   | cat     | m    |
  | Bluffy   | Harold | dog     | f    |
  | Fang     | Benny  | dog     | m    |
  | Bowser   | Diana  | dog     | m    |
  | Chirpy   | Gwen   | bird    | f    |
  | Whistler | Gwen   | bird    | N    |
  | Slim     | Benny  | snake   | m    |
  +----------+--------+---------+------+
  8 rows in set (0.00 sec)

DROP VIEWS
-----------

Syntax on dropping the views is as below.

::

  Name: 'DROP VIEW'
  Description:
  Syntax:
  DROP VIEW [IF EXISTS]
  view_name [, view_name] ...
  [RESTRICT | CASCADE]


Examples on dropping views

Example 1:

If you notice in this example , dropping the view is not going to delete the source table.

::

  mysql> select * from ms;
  +------+-------+
  | qty  | price |
  +------+-------+
  |   23 |    89 |
  +------+-------+
  1 row in set (0.00 sec)
  
  mysql> desc ms;
  +-------+---------+------+-----+---------+-------+
  | Field | Type    | Null | Key | Default | Extra |
  +-------+---------+------+-----+---------+-------+
  | qty   | int(11) | YES  |     | NULL    |       |
  | price | int(11) | YES  |     | NULL    |       |
  +-------+---------+------+-----+---------+-------+
  2 rows in set (0.00 sec)
  
  mysql> drop view msv;
  Query OK, 0 rows affected (0.00 sec)
  
  mysql> select * from msv;
  ERROR 1146 (42S02): Table 'new.msv' doesn't exist

  mysql> select * from ms;
  +------+-------+
  | qty  | price |
  +------+-------+
  |   23 |    89 |
  +------+-------+


Example 2:

If you notice in the below example , pet_view cannot be dropped as a table. We have to use **drop view** to drop the view.

::
  
  mysql> show tables like '%pet_view';
  +---------------------------+
  | Tables_in_new (%pet_view) |
  +---------------------------+
  | pet_view                  |
  +---------------------------+
  1 row in set (0.02 sec)
  
  mysql> drop tables pet_view;
  ERROR 1051 (42S02): Unknown table 'pet_view'
  mysql> 
  mysql> drop view pet_view;
  Query OK, 0 rows affected (0.00 sec)


Examples on Alter view:

::

  mysql> insert into ms values(23,89);
  Query OK, 1 row affected (0.03 sec)
  
  mysql> create view msv as select qty,price,qty+price as value from ms;
  Query OK, 0 rows affected (0.02 sec)
  
  mysql> select * from msv;
  +------+-------+-------+
  | qty  | price | value |
  +------+-------+-------+
  |   23 |    89 |   112 |
  |   23 |    89 |   112 |
  +------+-------+-------+
  2 rows in set (0.00 sec)
  
  mysql> alter view msv as select qty,price,qty+price as value from ms;
  Query OK, 0 rows affected (0.02 sec)
  
  mysql> select * from msv;
  +------+-------+-------+
  | qty  | price | value |
  +------+-------+-------+
  |   23 |    89 |   112 |
  |   23 |    89 |   112 |
  +------+-------+-------+
  2 rows in set (0.00 sec)

The negatives of using a view:
-------------------------------

* When the TEMPTABLE algorithm is used no index is used.
* They could hide a complex query and with querying the view could turn it into a sluggish query.
* Using the MERGE algorithm limits your view SELECT statement to basic querying only.
* You may choose the MERGE algorithm but if MySQL thinks it should use TEMPTABLE then it will change it.
* If your view does not have a one-to-one relationship with the table then it is not updatable.
* When you do add or change a table you will still need to update the CUD statements in your application.
* You cannot associate a trigger with a view.

The benefits of using a view:
--------------------------------

* Provide a useful data separation from application by containing the need to remember specific fields the query needs to filter by for all SELECT statements within the database.
* Tables can change over time and you need to add some new fields that you intend to filter by for most SELECT queries. All that needs to change is the SELECT statement with the new fields after altering the tables. Then updating the application will be easier as you may only need to remember to change it in a few places.
* They can make your SELECT queries more readable.

