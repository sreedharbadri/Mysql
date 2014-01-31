Introduction
-------------

There are several types of database relationships. Today we are going to cover the following:

• One to One relationships.
• One to many and many to many to one relationships.
• Many to Many relationships.
• Self referencing relationships.


One to One relationships
--------------------------

Lets take a example to understand the the one to one relationship.

::

  mysql> desc name;

  +-------------+-------------+------+-----+---------+-------+
  | Field       | Type        | Null | Key | Default | Extra |
  +-------------+-------------+------+-----+---------+-------+
  | id          | int(11)     | YES  |     | NULL    |       |
  | name        | varchar(10) | YES  |     | NULL    |       |
  | housenumber | int(11)     | YES  |     | NULL    |       |
  +-------------+-------------+------+-----+---------+-------+

  3 rows in set (0.00 sec)


  mysql> desc address;

  +-------------+---------+------+-----+---------+-------+
  | Field       | Type    | Null | Key | Default | Extra |
  +-------------+---------+------+-----+---------+-------+
  | housenumber | int(11) | YES  |     | NULL    |       |
  | address     | text    | YES  |     | NULL    |       |
  +-------------+---------+------+-----+---------+-------+

  2 rows in set (0.00 sec)


Lets try to insert values to understand the one to one relationship between both the tables.

::

  mysql> select * from name;

  +------+---------+-------------+
  | id   | name    | housenumber |
  +------+---------+-------------+
  |    1 | santosh |         101 |
  |    2 | dinesh  |         102 |
  |    3 | chanu   |         103 |
  +------+---------+-------------+

  3 rows in set (0.00 sec)


  mysql> select * from address;

  +-------------+-------------+
  | housenumber | address     |
  +-------------+-------------+
  |         101 | yousufguda  |
  |         102 | begumpet    |
  |         103 | rajumandary |
  +-------------+-------------+

  3 rows in set (0.01 sec)



Now we have a relationship between the name table and the Address table. If each address can belong to only one name, this relationship is “One to One”. Keep in mind that this kind of relationship is not very common. Our initial table that included the address along with the name could have worked fine in most cases.


Notice that now there is a field named “housenumber” in the name table, that refers to the matching record in the Address table. This is called a “Foreign Key” and it is used for all kinds of database relationships. We will cover this subject later in the article.

One to Many and Many to one Relationships
-------------------------------------------

This is most commonly used type of relationships. Lets take example of two table name and shopping table.

::

  mysql> desc name;

  +-------------+-------------+------+-----+---------+-------+
  | Field       | Type        | Null | Key | Default | Extra |
  +-------------+-------------+------+-----+---------+-------+
  | id          | int(11)     | YES  |     | NULL    |       |
  | name        | varchar(10) | YES  |     | NULL    |       |
  | housenumber | int(11)     | YES  |     | NULL    |       |
  +-------------+-------------+------+-----+---------+-------+

  3 rows in set (0.00 sec)

  mysql> desc shopping;

  +----------+------------+------+-----+---------+-------+
  | Field    | Type       | Null | Key | Default | Extra |
  +----------+------------+------+-----+---------+-------+
  | order_id | int(11)    | YES  |     | NULL    |       |
  | id       | int(11)    | YES  |     | NULL    |       |
  | amount   | tinyint(4) | YES  |     | NULL    |       |
  +----------+------------+------+-----+---------+-------+

  3 rows in set (0.01 sec)


  mysql> select * from name;

  +------+---------+-------------+
  | id   | name    | housenumber |
  +------+---------+-------------+
  |    1 | santosh |         101 |
  |    2 | dinesh  |         102 |
  |    3 | chanu   |         103 |
  +------+---------+-------------+

  3 rows in set (0.00 sec)


  mysql> select * from shopping;

  +----------+---------------+--------+
  | order_id | housenumber   | amount |
  +----------+---------------+--------+
  |        1 |        101    |    127 |
  |        2 |        101    |    110 |
  |        3 |        102    |    110 |
  +----------+---------------+--------+
  3 rows in set (0.00 sec)


An house number can do any number of orders. An order can contain any number of items.Items can have description in many languages.

Many to Many relationships
---------------------------

In some cases, you may need multiple instances on both sides of the relationship. For example, each order can contain multiple items. And each item can also be in multiple orders.

::

  mysql> desc shopping;

  +----------+------------+------+-----+---------+-------+
  | Field    | Type       | Null | Key | Default | Extra |
  +----------+------------+------+-----+---------+-------+
  | order_id | int(11)    | YES  |     | NULL    |       |
  | id       | int(11)    | YES  |     | NULL    |       |
  | amount   | tinyint(4) | YES  |     | NULL    |       |
  +----------+------------+------+-----+---------+-------+

  3 rows in set (0.01 sec)


  mysql> desc name;

  +-------------+-------------+------+-----+---------+-------+
  | Field       | Type        | Null | Key | Default | Extra |
  +-------------+-------------+------+-----+---------+-------+
  | id          | int(11)     | YES  |     | NULL    |       |
  | name        | varchar(10) | YES  |     | NULL    |       |
  | housenumber | int(11)     | YES  |     | NULL    |       |
  +-------------+-------------+------+-----+---------+-------+

  3 rows in set (0.00 sec)


  mysql> desc items;

  +-----------+----------+------+-----+---------+-------+
  | Field     | Type     | Null | Key | Default | Extra |
  +-----------+----------+------+-----+---------+-------+
  | order_id  | int(11)  | YES  |     | NULL    |       |
  | item_name | char(20) | YES  |     | NULL    |       |
  | item_D    | char(20) | YES  |     | NULL    |       |
  +-----------+----------+------+-----+---------+-------+

  3 rows in set (0.01 sec)


  mysql> desc item_orders;

  +----------+---------+------+-----+---------+-------+
  | Field    | Type    | Null | Key | Default | Extra |
  +----------+---------+------+-----+---------+-------+
  | order_id | int(11) | YES  |     | NULL    |       |
  | item_id  | int(11) | YES  |     | NULL    |       |
  +----------+---------+------+-----+---------+-------+

  2 rows in set (0.00 sec)


we can have many to many relationship between tables.

Self Referencing Relationships
-------------------------------------

This is used when a table needs to have a relationship with itself. We will cover an example on the following relationship in the self join examples.


