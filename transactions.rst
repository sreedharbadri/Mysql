mysql> desc t1;
+-------+------------+------+-----+---------+-------+
| Field | Type       | Null | Key | Default | Extra |
+-------+------------+------+-----+---------+-------+
| val   | tinyint(4) | YES  |     | NULL    |       |
+-------+------------+------+-----+---------+-------+
1 row in set (0.00 sec)

mysql> select * from t1;
+------+
| val  |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    5 |
|    6 |
+------+
6 rows in set (0.02 sec)

mysql> insert into t1 values (7);
Query OK, 1 row affected (0.00 sec)

mysql> rollback;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from t1;
+------+
| val  |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    5 |
|    6 |
|    7 |
+------+
7 rows in set (0.00 sec)

mysql> show create table t1;
+-------+--------------------------------------------------------------------------------------------+
| Table | Create Table                                                                               |
+-------+--------------------------------------------------------------------------------------------+
| t1    | CREATE TABLE `t1` (
  `val` tinyint(4) DEFAULT NULL
) ENGINE=MyISAM DEFAULT CHARSET=latin1 |
+-------+--------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> show variables like '%auto';
Empty set (0.00 sec)

mysql> set autocommit=0;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t1 values (8);
Query OK, 1 row affected (0.00 sec)

mysql> select * from t1;
+------+
| val  |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    5 |
|    6 |
|    7 |
|    8 |
+------+
8 rows in set (0.00 sec)

mysql> rollback;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> show warnings;
+---------+------+---------------------------------------------------------------+
| Level   | Code | Message                                                       |
+---------+------+---------------------------------------------------------------+
| Warning | 1196 | Some non-transactional changed tables couldn't be rolled back |
+---------+------+---------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> select * from t1;
+------+
| val  |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
|    5 |
|    6 |
|    7 |
|    8 |
+------+
8 rows in set (0.00 sec)

mysql> create table t2 ( val int ) engine=Innodb;
Query OK, 0 rows affected (0.08 sec)

mysql> set autocommit=0;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into table t2 values (1);
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'table t2 values (1)' at line 1
mysql> insert into table t2 values ('1');
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'table t2 values ('1')' at line 1
mysql> insert into  t2 values (1);
Query OK, 1 row affected (0.00 sec)

mysql> select * from t2;
+------+
| val  |
+------+
|    1 |
+------+
1 row in set (0.00 sec)

mysql> rollback;
Query OK, 0 rows affected (0.04 sec)

mysql> select * from t2;
Empty set (0.00 sec)

mysql> set autocommit=1;
Query OK, 0 rows affected (0.00 sec)


mysql> insert into t2 values (1);
Query OK, 1 row affected (0.00 sec)

mysql> insert into t2 values (2);
Query OK, 1 row affected (0.00 sec)

mysql> insert into t2 values (3);
Query OK, 1 row affected (0.00 sec)

mysql> insert into t2 values (4);
Query OK, 1 row affected (0.00 sec)

mysql> select * from t2;
+------+
| val  |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
+------+
4 rows in set (0.00 sec)

mysql> select database();
+------------+
| database() |
+------------+
| new        |
+------------+
1 row in set (0.00 sec)

mysql> commit;
Query OK, 0 rows affected (0.05 sec)

mysql> select * from t2;
+------+
| val  |
+------+
|    1 |
|    2 |
|    3 |
|    4 |
+------+
4 rows in set (0.00 sec)



