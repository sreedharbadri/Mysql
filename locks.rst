Introduction
-------------

Locking is a mechanism that prevents problems from occurring with simultaneous data access by multiple clients.Locks are managed by the server. It places a lock on data on behalf of one client to restrict access by other clients to the data until the lock has been released. The lock allows access to the data by the client thatholds the lock, but places limitations on what operations can be done by other clients that are contending for access.


The effect of locking is to serialize access to data so that when multiple clients want to perform conflicting operations, each must wait its turn. Not all type of concurrent access produce conflicts.If a client wants to read data, and other clients want to read the same data, do not produce a conflict, and they can all read at the same time. However, if another client wants to write, it must wait until the read has finished. If a client wants to write data, all other clients must wait until the write has finished. A reader must block writers, but not other readers. A writer must block both readers and writers.


A lock on data can be acquired implicitly or explicitly. For a client that does nothing special to acquire locks, the server implicitly acquires locks as necessary to process the client's statement safely. For example, the server acquires a read lock when the client issues a SELECT statement and a write lock when the client issues an INSERT statement.


Implicit locks are acquired only for the duration of a single statement. If implicit locking is not sufficient for a client's purposes, it can manage locks explicitly by acquiring them with LOCK TABLES and releasing them with UNLOCK TABLES.Explicit locking may be necessary when a client needs to perform an operation that spans multiple statements that must not be interrupted by other clients.


For example, an application might select a value from one table and then use it to determine which records to update in a set of other tables. With implicit locking, it's possible for another client to perform other, possibly conflicting changes between statements. To prevent this, the first client can place an explicit lock on the tables that it uses.


Another type of lock is advisory (cooperative) lock. Advisory locks do not lock data and they do not prevent access to data by other clients to the extent that theycooperate with each other. Unlike implicit and explicit lock, advisory locks are not managed by the server. Clients manage advisory locks using a set of function calls to cooperate among themselves.


Locking in MySQL occurs at different levels. Explicit locks acquired with LOCK TABLES are table-locks. For implicit locks, the lock level that the server uses depends on the storage engine:


MyISAM, MEMORY, and MERGE tables are locked at the table level.BDB tables are locked at the page level.
InnoDB tables are locked at the row level.


Deadlock cannot occur with table locking as it can with page or row locking. For example, with row-level locking, two clients might each acquire a lock on differentrows. If each then tries to modify the row that the other has locked, neither client can proceed. This is called a deadlock. With table locking, the server can determine what locks are needed and acquire them before executing a statement, so deadlock never occurs. An exception is possible when applications use cursors, because then the server must hold open a lock for a client across multiple statements. Suppose that client 1 opens a cursor for reading table t1 and client 2 opens a cursor for reading table t2. While the cursors are open, if each client tries to update the table being read by the other, deadlock can occur.


Explicit locking can improve performance for multiple statements executed as a group while the lock is in effect. First, less work is required by the server to acquire and release locks because it does not need to do so for each statement. It simply acquires all needed locks at the beginning of the operation, and release them at the end. Second, for statements that modify data, index flushing is reduced. If you execute multiple INSERT statements using implicit locking, index flushing occurs following each statement. If you lock the table explicitly and then perform all the inserts, index flushing occurs only once when you release the lock. This results in less disk activity.


The LOCK TABLES statement names each table to be locked and the type of lock to be acquired. The following statement acquires a read lock on the pet table and a write lock on the name table:

::

  LOCK TABLES pet READ, name WRITE;

To use LOCK TABLES, you must have LOCK TABLES privilege and SELECT privilege for each table to be locked.
If any of the tables to be locked are already in use, LOCK TABLES blocks until it has acquired all of the requested locks.


If you need to use multiple tables while holding an explicit lock, you must lock all of them at the same time because you cannot use any unlocked tables while you hold explicit lock.You must lock all the tables with a single LOCK TABLES statement.


LOCK TABLES releases any locks that you already hold, so you cannot use it multiple times to acquire multiple locks.

READ LOCK
-------------

READ locks locks a table for reading. It does not allow write operations even by the client that holds the lock. When a table is locked for reading, other clients can read from the table at the same time, but no client can write to it. A client that wants to write to a table that is read-locked must wait until all clients reading from it have finished and released their locks.


Lets go with a example:
open two connections , lets use our table name.

CONNECTION 1:

::

  mysql> lock tables name read;
  Query OK, 0 rows affected (0.00 sec)

  mysql> select * from name;
  
  +------+---------+-------------+
  | id   | name    | housenumber |
  +------+---------+-------------+
  |    1 | santosh |         101 |
  |    2 | dinesh  |         102 |
  |    3 | chanu   |         103 |
  +------+---------+-------------+
  3 rows in set (0.00 sec)


CONNECTION 2:

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

  mysql> insert into name values ('4','hari','104');
 
It just hangs, it will only work if you open lock in connection1.

WRITE LOCK
---------------

WRITE locks locks a table for writing. A WRITE lock is an exclusive lock. It can be acquired only when a table is not being used. Once acquired, only the client holding the write-lock can read from or write to the table. Other clients can neither read from nor write to it. No other client can lock the table for either reading or writing.

For implementing this lets create a user 'santosh'.

::

  mysql> create user santosh;
  Query OK, 0 rows affected (0.00 sec)
  
  mysql> grant select  on new.temp_table to 'santosh'@'localhost' identified by 'santosh';
  Query OK, 0 rows affected (0.00 sec)


CONNECTION 1:

::

  mysql> lock tables name write;
  Query OK, 0 rows affected (0.00 sec)

  mysql> select * from name;

  +------+---------+-------------+
  | id   | name    | housenumber |
  +------+---------+-------------+
  |    1 | santosh |         101 |
  |    2 | dinesh  |         102 |
  |    3 | chanu   |         103 |
  |    4 | hari    |         104 |
  +------+---------+-------------+
  4 rows in set (0.00 sec)

CONNECTION 2:

I logged in as user santosh.

::


  mysql> show tables;

  +---------------+
  | Tables_in_new |
  +---------------+
  | temp_table    |
  +---------------+

  1 row in set (0.00 sec)

  mysql> select * from temp_table;
  It hangs as the table is set to write lock.

READ LOCAL
-----------

READ LOCAL locks locks a table for reading, but allows concurrent inserts. A concurrent insert is an exception to the "readers block writers" principle. It applies only to MyISAM tables. If a MyISAM table has no holes in the middle resulting from deleted or updated records, inserts always take place at the end of the table. In that case, a client that is reading from a table can lock it with a READ LOCAL lock to allow other clients to insert into the table while the client holding the read lock reads from it. If a MyISAM table does have holes, you can remove them by using OPTIMIZE TABLE to defragment the table. You can acquire a READ LOCAL lock for a fragmented MyISAM table, or for a non-MyISAM table, but in such cases, concurrent inserts are not allowed. The lock acts like a regular READ lock.


CONNECTION 1:

::

  mysql> lock table temp_table read local;
  Query OK, 0 rows affected (0.00 sec)

  mysql> select * from temp_table;

  +------+
  | id   |
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


CONNECTION 2:

::

  mysql> insert into temp_table values ('7');
  Query OK, 1 row affected (0.00 sec)

  mysql> select * from temp_table;

  +------+
  | id   |
  +------+
  |    1 |
  |    2 |
  |    3 |
  |    4 |
  |    5 |
  |    6 |
  |    7 |
  |    7 |
  +------+
  8 rows in set (0.00 sec)

LOW_PRIORITY WRITE
--------------------

LOW_PRIORITY WRITE locks a table for writing, but acquires that lock with a lower priority. That is, if the client must wait for the lock, other clients that request read locks during the wait are allowed to get their locks first. A normal write lock request is satisfied when no other clients are using the table. If other clients are using the table when the request is made, it waits until those clients have finished. A LOW_PRIORITY WRITE lock request also waits for any new read requests that arrive while the lock request is pending.

CONNECTION 1:

::

  mysql> lock table temp_table low_priority write;
  Query OK, 0 rows affected (0.00 sec)

  mysql> select * from temp_table;
  
  +------+
  | id   |
  +------+
  |    1 |
  |    2 |
  |    3 |
  |    4 |
  |    5 |
  |    6 |
  |    7 |
  |    7 |
  +------+
  8 rows in set (0.00 sec)


CONNECTION 2:

UNLOCK TABLES
--------------------

To release explicit locks, issue an UNLOCK TABLES statement. This statement name no tables, because it releases all explicit locks held by the issuing client.
Explicit locks held by a client are also releases if the client issue another LOCK TABLES statement.


Locks can not be maintained across connections. If a client has any unreleased locks when its connection to the server terminates, the server implicitly releases its locks.An administrator with SUPER privilege can terminate a client connection with the KILL statement, which causes release of locks held by the client.


Table locks may be affected by transactions and vice versa. Beginning a transaction with START TRANSACTION causes an implicit UNLOCK TABLES. Issuing a LOCK TABLES statement will implicitly commit any pending transaction. If you have locked any tables, issuing an UNLOCK TABLES statement will implicitly commit any pending transaction.

