Introduction
--------------

Joins are an integral part of a relational database. Joins allow the database user to take advantage of the  relationships that were developed in the design phase of the database. A join is the term used to describe the act when two or more tables are "joined" together to retrieve needed data based on the relationships that  are shared between them.The JOIN statement of SQL enables you to design smaller, more specific tables that are easier to maintain than larger tables.

I am using couple of table to understand the example.

mysql> desc phone_call;

+----------+-------------+------+-----+---------+-------+

| Field    | Type        | Null | Key | Default | Extra |

+----------+-------------+------+-----+---------+-------+

| pid      | int(2)      | YES  |     | NULL    |       |

| spid     | int(2)      | YES  |     | NULL    |       |

| calledto | varchar(15) | YES  |     | NULL    |       |

+----------+-------------+------+-----+---------+-------+

3 rows in set (0.00 sec)



mysql> desc phone_people;

+-------+-------------+------+-----+---------+-------+

| Field | Type        | Null | Key | Default | Extra |

+-------+-------------+------+-----+---------+-------+

| name  | varchar(10) | YES  |     | NULL    |       |

| phone | varchar(10) | YES  |     | NULL    |       |

| pid   | int(2)      | YES  |     | NULL    |       |

+-------+-------------+------+-----+---------+-------+

3 rows in set (0.00 sec)


Let me try populating these tables with some values;

mysql> select * from phone_call;

+------+------+-----------------+

| pid  | spid | calledto        |

+------+------+-----------------+

|    1 |    1 | called to uncle |

|    3 |    2 | called to uncle |

|    3 |    3 | called to aunty |

|    3 |    4 | sms to uncle    |

|    4 |    5 | sms to aunty    |

+------+------+-----------------+

5 rows in set (0.00 sec)



mysql> select * from phone_people;

+------------+------------+------+

| name       | phone      | pid  |

+------------+------------+------+

| Mr santosh | 9703206361 |    1 |

| Mr hari    | 9861006360 |    2 |

| Mr dinesh  | 9885935780 |    3 |

+------------+------------+------+

3 rows in set (0.00 sec)

NATURAL JOIN

With this kind of JOIN query, the tables need to have a matching column name. In our case, both the tables have the pid column. So, MySQL will join the records only when the value of this column is matching on two records.

Note: I have changed the tables name in both cases on both sides of join.

mysql> select * from phone_people natural join  phone_call;

+------+------------+------------+------+-----------------+

| pid  | name       | phone      | spid | calledto        |

+------+------------+------------+------+-----------------+

|    1 | Mr santosh | 9703206361 |    1 | called to uncle |

|    3 | Mr dinesh  | 9885935780 |    2 | called to uncle |

|    3 | Mr dinesh  | 9885935780 |    3 | called to aunty |

|    3 | Mr dinesh  | 9885935780 |    4 | sms to uncle    |

+------+------------+------------+------+-----------------+

4 rows in set (0.00 sec)


mysql> select * from phone_call natural join  phone_people;

+------+------+-----------------+------------+------------+

| pid  | spid | calledto        | name       | phone      |

+------+------+-----------------+------------+------------+

|    1 |    1 | called to uncle | Mr santosh | 9703206361 |

|    3 |    2 | called to uncle | Mr dinesh  | 9885935780 |

|    3 |    3 | called to aunty | Mr dinesh  | 9885935780 |

|    3 |    4 | sms to uncle    | Mr dinesh  | 9885935780 |

+------+------+-----------------+------------+------------+

4 rows in set (0.00 sec)


CROSS JOIN/CARTESIAN JOIN

A close analysis of the results show that each row in phone_people is combined with each row in phone_call table. A cross join in not normally usefull as other joins , but this join does illustate the basic combining property of the all joins.

mysql> select * from phone_people,phone_call;

+------------+------------+------+------+------+-----------------+

| name       | phone      | pid  | pid  | spid | calledto        |

+------------+------------+------+------+------+-----------------+

| Mr santosh | 9703206361 |    1 |    1 |    1 | called to uncle |

| Mr hari    | 9861006360 |    2 |    1 |    1 | called to uncle |

| Mr dinesh  | 9885935780 |    3 |    1 |    1 | called to uncle |

| Mr santosh | 9703206361 |    1 |    3 |    2 | called to uncle |

| Mr hari    | 9861006360 |    2 |    3 |    2 | called to uncle |

| Mr dinesh  | 9885935780 |    3 |    3 |    2 | called to uncle |

| Mr santosh | 9703206361 |    1 |    3 |    3 | called to aunty |

| Mr hari    | 9861006360 |    2 |    3 |    3 | called to aunty |

| Mr dinesh  | 9885935780 |    3 |    3 |    3 | called to aunty |

| Mr santosh | 9703206361 |    1 |    3 |    4 | sms to uncle    |

| Mr hari    | 9861006360 |    2 |    3 |    4 | sms to uncle    |

| Mr dinesh  | 9885935780 |    3 |    3 |    4 | sms to uncle    |

| Mr santosh | 9703206361 |    1 |    4 |    5 | sms to aunty    |

| Mr hari    | 9861006360 |    2 |    4 |    5 | sms to aunty    |

| Mr dinesh  | 9885935780 |    3 |    4 |    5 | sms to aunty    |

+------------+------------+------+------+------+-----------------+

15 rows in set (0.00 sec)


we can run the above query using the join option too.

mysql> select * from phone_people cross join phone_call;

+------------+------------+------+------+------+-----------------+

| name       | phone      | pid  | pid  | spid | calledto        |

+------------+------------+------+------+------+-----------------+

| Mr santosh | 9703206361 |    1 |    1 |    1 | called to uncle |

| Mr hari    | 9861006360 |    2 |    1 |    1 | called to uncle |

| Mr dinesh  | 9885935780 |    3 |    1 |    1 | called to uncle |

| Mr santosh | 9703206361 |    1 |    3 |    2 | called to uncle |

| Mr hari    | 9861006360 |    2 |    3 |    2 | called to uncle |

| Mr dinesh  | 9885935780 |    3 |    3 |    2 | called to uncle |

| Mr santosh | 9703206361 |    1 |    3 |    3 | called to aunty |

| Mr hari    | 9861006360 |    2 |    3 |    3 | called to aunty |

| Mr dinesh  | 9885935780 |    3 |    3 |    3 | called to aunty |

| Mr santosh | 9703206361 |    1 |    3 |    4 | sms to uncle    |

| Mr hari    | 9861006360 |    2 |    3 |    4 | sms to uncle    |

| Mr dinesh  | 9885935780 |    3 |    3 |    4 | sms to uncle    |

| Mr santosh | 9703206361 |    1 |    4 |    5 | sms to aunty    |

| Mr hari    | 9861006360 |    2 |    4 |    5 | sms to aunty    |

| Mr dinesh  | 9885935780 |    3 |    4 |    5 | sms to aunty    |

+------------+------------+------+------+------+-----------------+

15 rows in set (0.00 sec)


INNER JOIN

mysql> select name,phone,calledto from phone_people,phone_call where phone_people.pid = phone_call.pid;

+------------+------------+-----------------+

| name       | phone      | calledto        |

+------------+------------+-----------------+

| Mr santosh | 9703206361 | called to uncle |

| Mr dinesh  | 9885935780 | called to uncle |

| Mr dinesh  | 9885935780 | called to aunty |

| Mr dinesh  | 9885935780 | sms to uncle    |

+------------+------------+-----------------+

4 rows in set (0.00 sec)



select name,phone,calledto from phone_people join phone_call on phone_people.pid = phone_call.pid;

mysql> select name,phone,calledto from phone_people join phone_call on phone_people.pid = phone_call.pid;

+------------+------------+-----------------+

| name       | phone      | calledto        |

+------------+------------+-----------------+

| Mr santosh | 9703206361 | called to uncle |

| Mr dinesh  | 9885935780 | called to uncle |

| Mr dinesh  | 9885935780 | called to aunty |

| Mr dinesh  | 9885935780 | sms to uncle    |

+------------+------------+-----------------+

4 rows in set (0.00 sec)



LEFT JOIN

If I do a LEFT JOIN, I get all records that match in the same way and IN ADDITION I get an extra record for each unmatched record in the left table of the join - thus ensuring (in my example) that every PERSON gets a mention.

select name,phone,calledto from phone_people left join phone_call on phone_people.pid = phone_call.pid;

+------------+------------+-----------------+

| name       | phone      | calledto        |

+------------+------------+-----------------+

| Mr santosh | 9703206361 | called to uncle |

| Mr hari    | 9861006360 | NULL            |

| Mr dinesh  | 9885935780 | called to uncle |

| Mr dinesh  | 9885935780 | called to aunty |

| Mr dinesh  | 9885935780 | sms to uncle    |

+------------+------------+-----------------+

5 rows in set (0.00 sec)


An OUTER may be added to a left or right , its provides for ODBC compatibility and doesn't add an extra capabilities.

mysql> select name,phone,calledto from phone_people  left outer join phone_call on phone_people.pid = phone_call.pid;

+------------+------------+-----------------+

| name       | phone      | calledto        |

+------------+------------+-----------------+

| Mr santosh | 9703206361 | called to uncle |

| Mr hari    | 9861006360 | NULL            |

| Mr dinesh  | 9885935780 | called to uncle |

| Mr dinesh  | 9885935780 | called to aunty |

| Mr dinesh  | 9885935780 | sms to uncle    |

+------------+------------+-----------------+

5 rows in set (0.00 sec)



NATURAL LEFT JOIN

The NATURAL LEFT JOIN is the same as a regular LEFT JOIN except that it automatically uses all
 the matching columns as part of the join. It is syntactically equivalent to a LEFT JOIN with a USING
 clause that names all the identical columns of the two tables


mysql> select name,phone,calledto from phone_people natural left join phone_call; 

+------------+------------+-----------------+

| name       | phone      | calledto        |

+------------+------------+-----------------+

| Mr santosh | 9703206361 | called to uncle |

| Mr hari    | 9861006360 | NULL            |

| Mr dinesh  | 9885935780 | called to uncle |

| Mr dinesh  | 9885935780 | called to aunty |

| Mr dinesh  | 9885935780 | sms to uncle    |

+------------+------------+-----------------+

5 rows in set (0.00 sec)


RIGHT JOIN

If I do a RIGHT JOIN, I get all the records that match and IN ADDITION I get an extra record for each unmatched record in the right table of the join.

mysql> select name,phone,calledto from phone_people right join phone_call on phone_people.pid = phone_call.pid;

+------------+------------+-----------------+

| name       | phone      | calledto        |

+------------+------------+-----------------+

| Mr santosh | 9703206361 | called to uncle |

| Mr dinesh  | 9885935780 | called to uncle |

| Mr dinesh  | 9885935780 | called to aunty |

| Mr dinesh  | 9885935780 | sms to uncle    |

| NULL       | NULL       | sms to aunty    |

+------------+------------+-----------------+

5 rows in set (0.00 sec)


An OUTER may be added to a left or right , its provides for ODBC compatibility and doesn't add an extra capabilities.

mysql> select name,phone,calledto from phone_people right outer join phone_call on phone_people.pid = phone_call.pid;

+------------+------------+-----------------+

| name       | phone      | calledto        |

+------------+------------+-----------------+

| Mr santosh | 9703206361 | called to uncle |

| Mr dinesh  | 9885935780 | called to uncle |

| Mr dinesh  | 9885935780 | called to aunty |

| Mr dinesh  | 9885935780 | sms to uncle    |

| NULL       | NULL       | sms to aunty    |

+------------+------------+-----------------+

5 rows in set (0.00 sec)


EQUI JOINS/NON EQUI JOINS

Because SQL supports an equi-join, you might assume that SQL also has a non-equi-join.
You would be right! Whereas the equi-join uses an = sign in the WHERE statement, the
 non-equi-join uses everything but an = sign.

JOINING A TABLE TO ITSELF

Lets take a quick example here with table pet.

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


Contents of the file pet.

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

8 rows in set (0.00 sec)


To find breeding pairs among your pets, you can join the pet table with itself to produce candidate pairs of males and females of like species.

mysql> SELECT p1.name, p1.sex, p2.name, p2.sex, p1.species FROM pet AS p1 INNER JOIN pet AS p2 ON p1.species = p2.species AND p1.sex = 'f' AND p2.sex = 'm';

+--------+------+--------+------+---------+

| name   | sex  | name   | sex  | species |

+--------+------+--------+------+---------+

| Fluffy | f    | Claws  | m    | cat     |

| Bluffy | f    | Fang   | m    | dog     |

| Bluffy | f    | Bowser | m    | dog     |

+--------+------+--------+------+---------+

3 rows in set (0.02 sec)



Hand picked examples to understand the joins:

I created the tables bdg,res and dom.

mysql> create table bdg (name text, bid int primary key);
Query OK, 0 rows affected (0.11 sec)

mysql> create table res (person text, bid int, rid int primary key);
Query OK, 0 rows affected (0.12 sec)


mysql> create table dom (domain text, rid int, did int primary key);
Query OK, 0 rows affected (0.08 sec)

Insert values into the tables:

mysql> insert into bdg values ("404",1);
Query OK, 1 row affected (0.04 sec)


mysql> insert into res values ("Graham",1,101);
Query OK, 1 row affected (0.03 sec)

mysql> insert into dom values ("www.grahamellis.co.uk",101,201);
Query OK, 1 row affected (0.03 sec)


mysql> insert into dom values ("www.sheepbingo.co.uk",101,202);
Query OK, 1 row affected (0.04 sec)

mysql> insert into res values ("Lisa",1,102);
Query OK, 1 row affected (0.03 sec)



mysql> insert into bdg values ("405",2);
Query OK, 1 row affected (0.05 sec)


Verify the values:

mysql> select * from bdg;

+------+-----+

| name | bid |

+------+-----+

| 404  |   1 |

| 405  |   2 |

+------+-----+

2 rows in set (0.00 sec)



mysql> select * from res;

+--------+------+-----+
| person | bid  | rid |
+--------+------+-----+
| Graham |    1 | 101 |
| Lisa   |    1 | 102 |
+--------+------+-----+

2 rows in set (0.00 sec)



mysql> select * from dom;

+-----------------------+------+-----+
| domain                | rid  | did |
+-----------------------+------+-----+
| www.grahamellis.co.uk |  101 | 201 |
| www.sheepbingo.co.uk  |  101 | 202 |
+-----------------------+------+-----+

2 rows in set (0.00 sec)


Few examples using the joins:

mysql> select * from bdg, res where bdg.bid = res.bid ;

+------+-----+--------+------+-----+
| name | bid | person | bid  | rid |
+------+-----+--------+------+-----+
| 404  |   1 | Graham |    1 | 101 |
| 404  |   1 | Lisa   |    1 | 102 |
+------+-----+--------+------+-----+

2 rows in set (0.00 sec)


mysql> select * from bdg, res, dom where bdg.bid = res.bid and  res.rid = dom.rid;

+------+-----+--------+------+-----+-----------------------+------+-----+

| name | bid | person | bid  | rid | domain                | rid  | did |

+------+-----+--------+------+-----+-----------------------+------+-----+

| 404  |   1 | Graham |    1 | 101 | www.grahamellis.co.uk |  101 | 201 |

| 404  |   1 | Graham |    1 | 101 | www.sheepbingo.co.uk  |  101 | 202 |

+------+-----+--------+------+-----+-----------------------+------+-----+

2 rows in set (0.00 sec)


Example of left join:

mysql> select * from bdg left join res on bdg.bid = res.bid ;

+------+-----+--------+------+------+

| name | bid | person | bid  | rid  |

+------+-----+--------+------+------+

| 404  |   1 | Graham |    1 |  101 |

| 404  |   1 | Lisa   |    1 |  102 |

| 405  |   2 | NULL   | NULL | NULL |

+------+-----+--------+------+------+

3 rows in set (0.00 sec)


mysql> select * from (bdg left join res on bdg.bid = res.bid)     left join dom on res.rid = dom.rid;

+------+-----+--------+------+------+-----------------------+------+------+

| name | bid | person | bid  | rid  | domain                | rid  | did  |

+------+-----+--------+------+------+-----------------------+------+------+

| 404  |   1 | Graham |    1 |  101 | www.grahamellis.co.uk |  101 |  201 |

| 404  |   1 | Graham |    1 |  101 | www.sheepbingo.co.uk  |  101 |  202 |

| 404  |   1 | Lisa   |    1 |  102 | NULL                  | NULL | NULL |

| 405  |   2 | NULL   | NULL | NULL | NULL                  | NULL | NULL |

+------+-----+--------+------+------+-----------------------+------+------+

4 rows in set (0.00 sec)


mysql> select * from (bdg left join res on bdg.bid = res.bid)     left join dom on res.rid = dom.rid where dom.rid is NULL;

+------+-----+--------+------+------+--------+------+------+

| name | bid | person | bid  | rid  | domain | rid  | did  |

+------+-----+--------+------+------+--------+------+------+

| 404  |   1 | Lisa   |    1 |  102 | NULL   | NULL | NULL |

| 405  |   2 | NULL   | NULL | NULL | NULL   | NULL | NULL |

+------+-----+--------+------+------+--------+------+------+

2 rows in set (0.00 sec)


mysql> select * from (bdg left join res on bdg.bid = res.bid)     left join dom on res.rid = dom.rid where res.rid is NULL;

+------+-----+--------+------+------+--------+------+------+

| name | bid | person | bid  | rid  | domain | rid  | did  |

+------+-----+--------+------+------+--------+------+------+

| 405  |   2 | NULL   | NULL | NULL | NULL   | NULL | NULL |

+------+-----+--------+------+------+--------+------+------+

1 row in set (0.00 sec)


mysql> select * from (bdg left join res on bdg.bid = res.bid)     left join dom on res.rid = dom.rid     where dom.rid is NULL and res.rid is not NULL;

+------+-----+--------+------+------+--------+------+------+

| name | bid | person | bid  | rid  | domain | rid  | did  |

+------+-----+--------+------+------+--------+------+------+

| 404  |   1 | Lisa   |    1 |  102 | NULL   | NULL | NULL |

+------+-----+--------+------+------+--------+------+------+

1 row in set (0.00 sec)


