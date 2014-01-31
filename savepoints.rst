mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> savepoint a;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t2 value (7);
Query OK, 1 row affected (0.00 sec)

mysql> insert into t2 value (8);
Query OK, 1 row affected (0.00 sec)

mysql> insert into t2 value (9);
Query OK, 1 row affected (0.00 sec)

mysql> select * from t2;
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
|    9 |
+------+
9 rows in set (0.00 sec)

mysql> rollback to a;
Query OK, 0 rows affected (0.04 sec)

mysql> select * from t2;
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
6 rows in set (0.00 sec)

mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> savepoint a;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t2 values (7);
Query OK, 1 row affected (0.00 sec)

mysql> insert into t2 values (8);
Query OK, 1 row affected (0.00 sec)

mysql> insert into t2 values (9);
Query OK, 1 row affected (0.00 sec)

mysql> savepoint b;
Query OK, 0 rows affected (0.00 sec)

mysql> insert into t2 values (10);
Query OK, 1 row affected (0.00 sec)

mysql> insert into t2 values (11);
Query OK, 1 row affected (0.00 sec)

mysql> insert into t2 values (12);
Query OK, 1 row affected (0.00 sec)

mysql> select * from t2;
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
|    9 |
|   10 |
|   11 |
|   12 |
+------+
12 rows in set (0.00 sec)

mysql> commit
    -> ;
Query OK, 0 rows affected (0.04 sec)


