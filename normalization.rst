Introduction
--------------


Normalization is a systematic approach of decomposing tables to eliminate data redundancy and undesirable characteristics like Insertion, Update and Deletion Anomalies. It is a two step process that puts data into tabular form by removing duplicated data from the relation tables.

Normalization is used for mainly two purpose,

1) Eliminating reduntant(useless) data.
2) Ensuring data dependencies make sense i.e data is logically stored.

Problems without Normalization?
---------------------------------

Without Normalization, it becomes difficult to handle and update the database, without facing data loss. Insertion, Updating and Deletion Anomalies are very frequent if database is not normalized. To understand these anomalies let us take an example of

Lets take a Vodafone connection table example.

Customer 
Name First 
Name  Second 
Name  Address1  Address2  Addon1
Addon1 cost Addon2  Addon2 cost Addon3  Addon3 cost Connection date
Connection plan Connection Type Receipt number  Bill generation date  Amount for month  Last month due

Normalization Rule
--------------------

First normalization form ( 1NF)
--------------------------------

A  row of data cannot contain repeating group of data i.e each column must have a unique value. Each row of data must have a unique identifier i.e Primary key. For example considering above table which is not in First normal form. Lets try to bring it to first normalization form. So in First Normalization lets try to remove the duplicate columns.
Customer info
Customer Name First Name  Second Name Address1
Address2      


Addon details
  Addon Cost  
      

Bill Details



Second Normal Form (2NF)
-------------------------

A table to be normalized to Second Normal Form should meet all the needs of First Normal Form and there must not be any partial dependency of any column on primary key. It means that for a table that has concatenated primary key, each column in the table that is not part of the primary key must depend upon the entire concatenated key for its existence. If any column depends oly on one part of the concatenated key, then the table fails Second normal form.


Third Normal Form (3NF)
-------------------------

Third Normal form applies that every non-prime attribute of table must be dependent on primary key. The transitive functional dependency should be removed from the table. The table must be in Second Normal form.


Boyce and Codd Normal Form (BCNF)
-----------------------------------

Boyce and Codd Normal Form is a higher version of the Third Normal form. This form deals with certain type of anamoly that is not handled by 3NF. A 3NF table which does not have multiple overlapping candidate keys is said to be in BCNF.

