Introduction
-------------

MySQL supports a number of data types in several categories: 

* Numeric types.
* Date and time types.
* String (character and byte) types. 

This chapter provides an overview of these data types, a more detailed description of the properties of the types in each category, and a summary of the data type storage requirements. The initial overview is intentionally brief. The more detailed descriptions later in the chapter should be consulted for additional informationabout particular data types, such as the permissible formats in which you can specify values. 

Numeric types
--------------

Numeric types are meant to store numbers only.The numeric type can be broken down further.For example, there are whole numbers and fractions as well as negative andpositive values. Different numeric types take up a different amount of space in memory. The reason is that each type has a different range.

INT
^^^^

A normal-sized integer that can be signed or unsigned. If signed, the allowable range is from -2147483648 to 2147483647. If unsigned, the allowable range is from 0 to 4294967295. You can specify a width of up to 11 digits.

TINYINT
^^^^^^^^^

A very small integer that can be signed or unsigned. If signed, the allowable range is from -128 to 127. If unsigned, the allowable range is from 0 to 255. You can specify a width of up to 4 digits.

SMALLINT
^^^^^^^^^

A small integer that can be signed or unsigned. If signed, the allowable range is from -32768 to 32767. If unsigned, the allowable range is from 0 to 65535. You canspecify a width of up to 5 digits.

MEDIUMINT
^^^^^^^^^

A medium-sized integer that can be signed or unsigned. If signed, the allowable range is from -8388608 to 8388607. If unsigned, the allowable range is from 0 to 16777215. You can specify a width of up to 9 digits.

BIGINT
^^^^^^^^^

A large integer that can be signed or unsigned. If signed, the allowable range is from -9223372036854775808 to 9223372036854775807. If unsigned, the allowable rangeis from 0 to 18446744073709551615. You can specify a width of up to 11 digits.

FLOAT
^^^^^^^^^

A floating-point number that cannot be unsigned. You can define the display length (M) and the number of decimals (D). This is not required and will default to 10,2, where 2 is the number of decimals and 10 is the total number of digits (including decimals). Decimal precision can go to 24 places for a FLOAT.


DOUBLE
^^^^^^^^^

A double precision floating-point number that cannot be unsigned. You can define the display length (M) and the number of decimals (D). This is not required and will default to 16,4, where 4 is the number of decimals. Decimal precision can go to 53 places for a DOUBLE. REAL is a synonym for DOUBLE.

DECIMAL
^^^^^^^^^

An unpacked floating-point number that cannot be unsigned. In unpacked decimals, each decimal corresponds to one byte. Defining the display length (M) and the number of decimals (D) is required. NUMERIC is a synonym for DECIMAL.

Date and Time Types
---------------------

DATE
^^^^^^^^^

A date in YYYY-MM-DD format, between 1000-01-01 and 9999-12-31. For example, December 30th, 1973 would be stored as 1973-12-30.

DATATIME
^^^^^^^^^

A date and time combination in YYYY-MM-DD HH:MM:SS format, between 1000-01-01 00:00:00 and 9999-12-31 23:59:59. For example, 3:30 in the afternoon on December 30th,1973 would be stored as 1973-12-30 15:30:00.

TIMESTAMP
^^^^^^^^^^^

A timestamp between midnight, January 1, 1970 and sometime in 2037. This looks like the previous DATETIME format, only without the hyphens between numbers; 3:30 in the afternoon on December 30th, 1973 would be stored as 19731230153000 ( YYYYMMDDHHMMSS ).

TIME
^^^^^^^^^^^

Stores the time in HH:MM:SS format.

YEAR
^^^^^^^^^^^

Stores a year in 2-digit or 4-digit format. If the length is specified as 2 (for example YEAR(2)), YEAR can be 1970 to 2069 (70 to 69). If the length is specified as 4, YEAR can be 1901 to 2155. The default length is 4.

String Types

CHAR
^^^^^^^^^^^

A fixed-length string between 1 and 255 characters in length (for example CHAR(5)), right-padded with spaces to the specified length when stored. Defining a length is not required, but the default is 1.

VARCHAR
^^^^^^^^^^^

A variable-length string between 1 and 255 characters in length; for example VARCHAR(25). You must define a length when creating a VARCHAR field.

BLOB OR TEXT
^^^^^^^^^^^^^^^

A field with a maximum length of 65535 characters. BLOBs are "Binary Large Objects" and are used to store large amounts of binary data, such as images or other types of files. Fields defined as TEXT also hold large amounts of data; the difference between the two is that sorts and comparisons on stored data are case sensitive on BLOBs and are not case sensitive in TEXT fields. You do not specify a length with BLOB or TEXT.

TINYBLOB OR TINYTEXT
^^^^^^^^^^^^^^^^^^^^^

A BLOB or TEXT column with a maximum length of 255 characters. You do not specify a length with TINYBLOB or TINYTEXT.

MEDIUMBLOB OR MEDIUMTEXT
^^^^^^^^^^^^^^^^^^^^^^^^^^

A BLOB or TEXT column with a maximum length of 16777215 characters. You do not specify a length with MEDIUMBLOB or MEDIUMTEXT.

LONGBLOB OR LONGTEXT
^^^^^^^^^^^^^^^^^^^^^^^^^^

A BLOB or TEXT column with a maximum length of 4294967295 characters. You do not specify a length with LONGBLOB or LONGTEXT.

ENUM
^^^^^^^^^^^^^^^^^^^^^^^^^^

An enumeration, which is a fancy term for list. When defining an ENUM, you are creating a list of items from which the value must be selected (or it can be NULL). For example, if you wanted your field to contain "A" or "B" or "C", you would define your ENUM as ENUM ('A', 'B', 'C') and only those values (or NULL) could ever populate that field.

::


  mysql> create table t1 ( var enum('y','n') default 'y');
  Query OK, 0 rows affected (0.10 sec)

  mysql> desc t1;
  +-------+---------------+------+-----+---------+-------+
  | Field | Type          | Null | Key | Default | Extra |
  +-------+---------------+------+-----+---------+-------+
  | var   | enum('y','n') | YES  |     | y       |       |
  +-------+---------------+------+-----+---------+-------+
  1 row in set (0.00 sec)

  mysql> insert into t1 values (1)
  -> ;
  Query OK, 1 row affected (0.00 sec)

  mysql> select * from t1;
  +------+
  | var  |
  +------+
  | y    |
  +------+
  1 row in set (0.00 sec)

  mysql> insert into t1 values ();
  Query OK, 1 row affected (0.00 sec)

  mysql> select * from t1;
  +------+
  | var  |
  +------+
  | y    |
  | y    |
  +------+
  2 rows in set (0.00 sec)

  mysql> insert into t1 values ('y,n');
  Query OK, 1 row affected, 1 warning (0.00 sec)

  mysql> show warnings;
  +---------+------+------------------------------------------+
  | Level   | Code | Message                                  |
  +---------+------+------------------------------------------+
  | Warning | 1265 | Data truncated for column 'var' at row 1 |
  +---------+------+------------------------------------------+
  1 row in set (0.00 sec)

  mysql> select * from t1;
  +------+
  | var  |
  +------+
  | y    |
  | y    |
  |      |
  +------+
  3 rows in set (0.00 sec)

SETS
^^^^^^

::


  mysql> create table t2 ( var set('item1','item2','item3'));
  Query OK, 0 rows affected (0.08 sec)

  mysql> desc t2;
  +-------+------------------------------+------+-----+---------+-------+
  | Field | Type                         | Null | Key | Default | Extra |
  +-------+------------------------------+------+-----+---------+-------+
  | var   | set('item1','item2','item3') | YES  |     | NULL    |       |
  +-------+------------------------------+------+-----+---------+-------+
  1 row in set (0.00 sec)

  mysql> insert into t2 values ('item1');
  Query OK, 1 row affected (0.00 sec)

  mysql> insert into t2 values ('item2');
  Query OK, 1 row affected (0.00 sec)

  mysql> insert into t2 values ('item2,item3');
  Query OK, 1 row affected (0.00 sec)

  mysql> select * from t2;
  +-------------+
  | var         |
  +-------------+
  | item1       |
  | item2       |
  | item2,item3 |
  +-------------+
  3 rows in set (0.00 sec)



ADDITIONAL COLUMN MODIFIERS
-------------------------------

MySQL has several key words that modify how a column acts. For example, you have already learned aboutthe AUTO_INCREMENT, UNSIGNED, and ZEROFILL modifiers and how they affect the column in which they are used. Some modifiers only apply to certain type columns.

ZEROFILL
^^^^^^^^^^^^^^^^^^^^^^^^^^

The ZEROFILL column modifier is used to display leading zeros of a number based on the display width. As mentioned earlier, all numeric types have an optional display width. For example, if you declare an INT(8) ZEROFILL, and the value you're storing is 23, it will be displayed as 00000023.

AUTO_INCREMENT
^^^^^^^^^^^^^^^^^^^^^^^^^^

The AUTO_INCREMENT column modifier automatically increases the value of a column by adding 1 to thecurrent maximum value. It provides a counter that is useful for creating unique values. The value of a newly inserted row into an AUTO_INCREMENT column starts at 1 and increases by 1 for every record that is inserted into the table. 

::


  mysql> create table t5 ( num tinyint AUTO_INCREMENT UNIQUE , name char(10));
  Query OK, 0 rows affected (0.10 sec)

  mysql> desc t5
    -> ;
  +-------+------------+------+-----+---------+----------------+
  | Field | Type       | Null | Key | Default | Extra          |
  +-------+------------+------+-----+---------+----------------+
  | num   | tinyint(4) | NO   | PRI | NULL    | auto_increment |
  | name  | char(10)   | YES  |     | NULL    |                |
  +-------+------------+------+-----+---------+----------------+
  2 rows in set (0.00 sec)

  mysql> create table t6 ( num tinyint unique not null auto_increment, name char(10));
  Query OK, 0 rows affected (0.08 sec)

  mysql> desc t6;
  +-------+------------+------+-----+---------+----------------+
  | Field | Type       | Null | Key | Default | Extra          |
  +-------+------------+------+-----+---------+----------------+
  | num   | tinyint(4) | NO   | PRI | NULL    | auto_increment |
  | name  | char(10)   | YES  |     | NULL    |                |
  +-------+------------+------+-----+---------+----------------+
  2 rows in set (0.00 sec)
  
  mysql> create table t7 ( num tinyint primary key auto_increment, name char(10));
  Query OK, 0 rows affected (0.09 sec)

  mysql> desc t7;
  +-------+------------+------+-----+---------+----------------+
  | Field | Type       | Null | Key | Default | Extra          |
  +-------+------------+------+-----+---------+----------------+
  | num   | tinyint(4) | NO   | PRI | NULL    | auto_increment |
  | name  | char(10)   | YES  |     | NULL    |                |
  +-------+------------+------+-----+---------+----------------+
  2 rows in set (0.00 sec)

  mysql> 
  
  mysql> create table t8 ( val tinyint unique auto_increment);
  Query OK, 0 rows affected (0.06 sec)

  mysql> desc t8;
  +-------+------------+------+-----+---------+----------------+
  | Field | Type       | Null | Key | Default | Extra          |
  +-------+------------+------+-----+---------+----------------+
  | val   | tinyint(4) | NO   | PRI | NULL    | auto_increment |
  +-------+------------+------+-----+---------+----------------+
  1 row in set (0.00 sec)

  mysql> insert into t8 values (1);
  Query OK, 1 row affected (0.00 sec)

  mysql> insert into t8 values ();
  Query OK, 1 row affected (0.00 sec)

  mysql> select * from t8;
  +-----+
  | val |
  +-----+
  |   1 |
  |   2 |
  +-----+
  2 rows in set (0.00 sec)

  mysql> insert into t8 values (NULL);
  Query OK, 1 row affected (0.00 sec)

  mysql> select * from t8;
  +-----+
  | val |
  +-----+
  |   1 |
  |   2 |
  |   3 |
  +-----+

  mysql> insert into t8 values (5);
  Query OK, 1 row affected (0.00 sec)

  mysql> insert into t8 values ();
  Query OK, 1 row affected (0.00 sec)

  mysql> insert into t8 values ();
  Query OK, 1 row affected (0.00 sec)

  mysql> select * from t8;
  +-----+
  | val |
  +-----+
  |   1 |
  |   2 |
  |   3 |
  |   5 |
  |   6 |
  |   7 |
  +-----+
  6 rows in set (0.00 sec)



BINARY
^^^^^^^^^^^^^^^^^^^^^^^^^^

The BINARY modifier causes the values stored in these types to treated as binary strings, making them case sensitive. When you sort or compare these strings, they will also take case into consideration. By default, VARCHAR and CHAR types are not stored as binary.

DEFAULT
^^^^^^^^^^^^^^^^^^^^^^^^^^

The DEFAULT modifier allows you to specify the value of a column if one does not exist.

Lets take a quick example here.

::

  mysql> create table t1 ( num tinyint NOT NULL default 0);
  Query OK, 0 rows affected (0.12 sec)

  mysql> desc t1;
  +-------+------------+------+-----+---------+-------+
  | Field | Type       | Null | Key | Default | Extra |
  +-------+------------+------+-----+---------+-------+
  | num   | tinyint(4) | NO   |     | 0       |       |
  +-------+------------+------+-----+---------+-------+
  1 row in set (0.00 sec)

  mysql> insert into t1 values (1);
  Query OK, 1 row affected (0.00 sec)

  mysql> insert into t1 values ();
  Query OK, 1 row affected (0.00 sec)

  mysql> select * from t1;
  +-----+
  | num |
  +-----+
  |   1 |
  |   0 |
  +-----+
  2 rows in set (0.00 sec)


NULL and NOT NULL
^^^^^^^^^^^^^^^^^^^^^^^^^^

The NULL and NOT NULL modifiers specify whether a column must have some sort of value in it. Forexample, if a column is defined as NOT NULL, a value must be placed in that column. Remember thatNULL is absolutely no value whatsoever. An empty string (" "), even though it looks like it is nothing, is NOT NULL. Using NULL and NOT NULL can force required constraints on the data that is being stored.

mysql> create table t2 ( num tinyint);
Query OK, 0 rows affected (0.07 sec)

::


   mysql> desc t2;
   +-------+------------+------+-----+---------+-------+
   | Field | Type       | Null | Key | Default | Extra |
   +-------+------------+------+-----+---------+-------+
   | num   | tinyint(4) | YES  |     | NULL    |       |
   +-------+------------+------+-----+---------+-------+
 
   mysql> select * from t2;
   +------+
   | num  |
   +------+
   |    1 |
   | NULL |
   +------+
   2 rows in set (0.00 sec)
 
   mysql> create table t3 ( num tinyint NULL default 0);
   Query OK, 0 rows affected (0.08 sec)
 
   mysql> desc t3;
   +-------+------------+------+-----+---------+-------+
   | Field | Type       | Null | Key | Default | Extra |
   +-------+------------+------+-----+---------+-------+
   | num   | tinyint(4) | YES  |     | 0       |       |
   +-------+------------+------+-----+---------+-------+
   1 row in set (0.00 sec)
 
   mysql> insert into t3 values (1);
   Query OK, 1 row affected (0.00 sec)
 
   mysql> insert into t3 values ();
   Query OK, 1 row affected (0.00 sec)
 
   mysql> insert into t3 values (NULL);
   Query OK, 1 row affected (0.00 sec)
 
   mysql> select * from t3;
   +------+
   | num  |
   +------+
   |    1 |
   |    0 |
   | NULL |
   +------+
   3 rows in set (0.00 sec)
 
   mysql> insert into t2 values (NULL);
   Query OK, 1 row affected (0.00 sec)
 
 
   mysql> select * from t2;
   +------+
   | num  |
   +------+
   |    1 |
   | NULL |
   | NULL |
   +------+
   3 rows in set (0.00 sec)
 
   mysql> insert into t1 values (NULL);
   ERROR 1048 (23000): Column 'num' cannot be null
 
   mysql> desc t1;
   +-------+------------+------+-----+---------+-------+
   | Field | Type       | Null | Key | Default | Extra |
   +-------+------------+------+-----+---------+-------+
   | num   | tinyint(4) | NO   |     | 0       |       |
   +-------+------------+------+-----+---------+-------+
   1 row in set (0.00 sec)
 



UNIQUE
^^^^^^^^^^^^^^^^^^^^^^^^^^

The UNIQUE modifier enforces the rule that all data within the declared column must be unique. If you try to insert a value that is not unique, an error will be generated.

::

  mysql> create table t3 ( num tinyint UNIQUE);
  Query OK, 0 rows affected (0.07 sec)

  mysql> desc t3;
  +-------+------------+------+-----+---------+-------+
  | Field | Type       | Null | Key | Default | Extra |
  +-------+------------+------+-----+---------+-------+
  | num   | tinyint(4) | YES  | UNI | NULL    |       |
  +-------+------------+------+-----+---------+-------+
  1 row in set (0.00 sec)

  mysql> insert into t3 values (1);
  Query OK, 1 row affected (0.00 sec)

  mysql> insert into t3 values (1);
  ERROR 1062 (23000): Duplicate entry '1' for key 'num'
  mysql> insert into t3 values ();
  Query OK, 1 row affected (0.00 sec)

  mysql> insert into t3 values ();
  Query OK, 1 row affected (0.00 sec)

  mysql> select * from t3;
  +------+
  | num  |
  +------+
  | NULL |
  | NULL |
  |    1 |
  +------+
  3 rows in set (0.00 sec)



PRIMARY KEY
^^^^^^^^^^^^^^^^^^^^^^^^^^

We will be covering this in the key sections completely.

