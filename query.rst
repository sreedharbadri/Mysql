Introduction
--------------

Lets start working with sql queries to get a better understanding about how the sql queries work.

Working with select
---------------------


Help on select.
----------------

::
  
  mysql> help 'select';

Selecting all data.
---------------------

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
  | Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
  | Slim     | Benny  | snake   | m    | 1996-04-29 | NULL       |
  +----------+--------+---------+------+------------+------------+
  8 rows in set (0.00 sec)


selecting particular rows
---------------------------

::

  mysql> select * from pet where owner='Harold';

  +--------+--------+---------+------+------------+-------+
  | name   | owner  | species | sex  | birth      | death |
  +--------+--------+---------+------+------------+-------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL  |
  | Bluffy | Harold | dog     | f    | 1989-05-13 | NULL  |
  +--------+--------+---------+------+------------+-------+
  2 rows in set (0.00 sec)

  mysql> select * from pet where sex='f';

  +--------+--------+---------+------+------------+-------+
  | name   | owner  | species | sex  | birth      | death |
  +--------+--------+---------+------+------------+-------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL  |
  | Bluffy | Harold | dog     | f    | 1989-05-13 | NULL  |
  | Chirpy | Gwen   | bird    | f    | 1998-09-11 | NULL  |
  +--------+--------+---------+------+------------+-------+
  
  mysql> select * from pet where sex='f' and species='bird';
  +--------+-------+---------+------+------------+-------+
  | name   | owner | species | sex  | birth      | death |
  +--------+-------+---------+------+------------+-------+
  | Chirpy | Gwen  | bird    | f    | 1998-09-11 | NULL  |
  +--------+-------+---------+------+------------+-------+
  1 row in set (0.01 sec)


  mysql> SELECT * FROM pet WHERE (species = 'cat' AND sex = 'm') OR (species = 'dog' AND sex = 'f');
  +--------+--------+---------+------+------------+-------+
  | name   | owner  | species | sex  | birth      | death |
  +--------+--------+---------+------+------------+-------+
  | Claws  | Gwen   | cat     | m    | 1994-03-17 | NULL  |
  | Bluffy | Harold | dog     | f    | 1989-05-13 | NULL  |
  +--------+--------+---------+------+------------+-------+
  2 rows in set (0.00 sec)



Selecting Particular Columns
-----------------------------

::

  mysql> select name,owner,species from pet where sex='f' and species='dog';
  
  +--------+--------+---------+
  | name   | owner  | species |
  +--------+--------+---------+
  | Bluffy | Harold | dog     |
  +--------+--------+---------+
  1 row in set (0.00 sec)

  mysql> select name,powner from pet;
  +----------+--------+
  | name     | powner |
  +----------+--------+
  | Fluffy   | Harold |
  | Claws    | Gwen   |   
  | Bluffy   | Harold |
  | Fang     | Benny  |
  | Bowser   | Diana  |
  | Chirpy   | Gwen   |   
  | Whistler | Gwen   |   
  | Slim     | Benny  |
  +----------+--------+
  8 rows in set (0.00 sec)

  mysql> select name as pet_name,powner as pet_owner from pet;
  +----------+-----------+
  | pet_name | pet_owner |
  +----------+-----------+
  | Fluffy   | Harold    |   
  | Claws    | Gwen      |   
  | Bluffy   | Harold    |   
  | Fang     | Benny     |   
  | Bowser   | Diana     |   
  | Chirpy   | Gwen      |   
  | Whistler | Gwen      |   
  | Slim     | Benny     |   
  +----------+-----------+
  8 rows in set (0.00 sec)

  mysql> select p.name,p.powner from pet as p;
  +----------+--------+
  | name     | powner |
  +----------+--------+
  | Fluffy   | Harold |
  | Claws    | Gwen   |
  | Bluffy   | Harold |
  | Fang     | Benny  |
  | Bowser   | Diana  |
  | Chirpy   | Gwen   |
  | Whistler | Gwen   |
  | Slim     | Benny  |
  +----------+--------+
  8 rows in set (0.00 sec)

  mysql> select name,powner from pet where name='fluffy';
  +--------+--------+
  | name   | powner |
  +--------+--------+
  | Fluffy | Harold |
  +--------+--------+
  1 row in set (0.00 sec)

  mysql> select name,powner from pet where name=binary('fluffy');
  Empty set (0.00 sec)


Comparison Operators
---------------------

Apart from the equality operator(=) there are other operators too which can be user with the WHERE clause.

+------------------------+---------------------------------+
| Operator               | Description                     |
+========================+=================================+
| =                      | Equal to                        |
+------------------------+---------------------------------+
|!=                      | Not equal to                    |
+------------------------+---------------------------------+
|<>                      | Not equal to                    |
+------------------------+---------------------------------+
|>                       |  Greater than                   |
+------------------------+---------------------------------+
|<                       |  Less than                      |
+------------------------+---------------------------------+
|<=                      |  Less than equl to              |
+------------------------+---------------------------------+
|>=                      | Greater than or equal to        |
+------------------------+---------------------------------+
|BETWEEN x AND y         |Falls between two values x and y |
+------------------------+---------------------------------+

Lets work on few examples of comparison operators.

::

  mysql> select name,owner from pet where death='1995-07-29';
  +--------+-------+
  | name   | owner |
  +--------+-------+
  | Bowser | Diana |
  +--------+-------+
  1 row in set (0.01 sec)

selecting a particular column with greater than equal to operator.

::

  mysql> select name,owner,birth from pet where birth >= '1990-08-27';
  +----------+--------+------------+
  | name     | owner  | birth      |
  +----------+--------+------------+
  | Fluffy   | Harold | 1992-02-04 |
  | Claws    | Gwen   | 1994-03-17 |
  | Fang     | Benny  | 1990-08-27 |
  | Chirpy   | Gwen   | 1998-09-11 |
  | Whistler | Gwen   | 1997-12-09 |
  | Slim     | Benny  | 1996-04-29 |
  +----------+--------+------------+
  6 rows in set (0.00 sec)

selecting a particular columns with less than or equal to operator.

::

  mysql> select name,owner,birth from pet where birth <= '1990-08-27';
  +--------+--------+------------+
  | name   | owner  | birth      |
  +--------+--------+------------+
  | Bluffy | Harold | 1989-05-13 |
  | Fang   | Benny  | 1990-08-27 |
  | Bowser | Diana  | 1979-08-31 |
  +--------+--------+------------+
  3 rows in set (0.00 sec)

selecting a particular column with not equal to operator.

::

  mysql> select name,owner,sex,species from pet where sex !='m' and sex !='f';
  +----------+-------+------+---------+
  | name     | owner | sex  | species |
  +----------+-------+------+---------+
  | Whistler | Gwen  | N    | bird    |
  +----------+-------+------+---------+
  1 row in set (0.01 sec)

Range values
-------------

The purpose of this operator is to find the values between the upper and lower range of values.selecting a particular column using BETWEEN operators.

::

  mysql> select * from pet where birth between '1990-08-27' and '1996-04-29';
  +--------+--------+---------+------+------------+-------+
  | name   | owner  | species | sex  | birth      | death |
  +--------+--------+---------+------+------------+-------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL  |
  | Claws  | Gwen   | cat     | m    | 1994-03-17 | NULL  |
  | Fang   | Benny  | dog     | m    | 1990-08-27 | NULL  |
  | Slim   | Benny  | snake   | m    | 1996-04-29 | NULL  |
  +--------+--------+---------+------+------------+-------+
  4 rows in set (0.00 sec)

Checking for null values
-------------------------

Sometimes we need to check for columns that contain no value.Columns that have no values are called as NULL values.

::

  mysql> select * from pet where death IS NULL;
  +----------+--------+---------+------+------------+-------+
  | name     | owner  | species | sex  | birth      | death |
  +----------+--------+---------+------+------------+-------+
  | Fluffy   | Harold | cat     | f    | 1992-02-04 | NULL  |
  | Claws    | Gwen   | cat     | m    | 1994-03-17 | NULL  |
  | Bluffy   | Harold | dog     | f    | 1989-05-13 | NULL  |
  | Fang     | Benny  | dog     | m    | 1990-08-27 | NULL  |
  | Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL  |
  | Whistler | Gwen   | bird    | N    | 1997-12-09 | NULL  |
  | Slim     | Benny  | snake   | m    | 1996-04-29 | NULL  |
  +----------+--------+---------+------+------------+-------+
  7 rows in set (0.00 sec)

OR operator
-------------

If you wanted to retrieve all the rows from a table where the product matched either one value or another. This can be achieved using the WHERE clause in combination with the OR operator. 

::

  mysql> select * from pet where species = 'cat' or species ='dog';
  +--------+--------+---------+------+------------+------------+
  | name   | owner  | species | sex  | birth      | death      |
  +--------+--------+---------+------+------------+------------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL       |
  | Claws  | Gwen   | cat     | m    | 1994-03-17 | NULL       |
  | Bluffy | Harold | dog     | f    | 1989-05-13 | NULL       |
  | Fang   | Benny  | dog     | m    | 1990-08-27 | NULL       |
  | Bowser | Diana  | dog     | m    | 1979-08-31 | 1995-07-29 |
  +--------+--------+---------+------+------------+------------+
  5 rows in set (0.00 sec)

AND operator
--------------

The AND operator selects rows based on the requirement that meet multiple requirements (as opposed to the "either or" approach of the OR operator).

::

  mysql> select * from pet where species ='cat' and sex='f';
  +--------+--------+---------+------+------------+-------+
  | name   | owner  | species | sex  | birth      | death |
  +--------+--------+---------+------+------------+-------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL  |
  +--------+--------+---------+------+------------+-------+
  1 row in set (0.00 sec)

Combining AND and OR
----------------------

A SELECT statement with a WHERE clause can combine any number of AND and OR operators to create complex filtering requirements.

::

  mysql> select * from pet where species = 'cat' or sex = 'f' and birth >= '1989-05-13';
  +--------+--------+---------+------+------------+-------+
  | name   | owner  | species | sex  | birth      | death |
  +--------+--------+---------+------+------------+-------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL  |
  | Claws  | Gwen   | cat     | m    | 1994-03-17 | NULL  |
  | Bluffy | Harold | dog     | f    | 1989-05-13 | NULL  |
  | Chirpy | Gwen   | bird    | f    | 1998-09-11 | NULL  |
  +--------+--------+---------+------+------------+-------+
  4 rows in set (0.00 sec)

Operator Precedence
---------------------

When combining operators it is important to understand something called operator precedence, which refers to the order in which operators in the same statement are evaluated. By default MySQL evaluates AND expressions before OR expressions regardless of whether the OR appears before the AND when reading the statement from leftto right.

We can see the difference between the queries in both the below cases.

::

  mysql> select * from pet where  species = 'dog' or sex = 'm' and death is  NOT NULL;
  +--------+--------+---------+------+------------+------------+
  | name   | owner  | species | sex  | birth      | death      |
  +--------+--------+---------+------+------------+------------+
  | Bluffy | Harold | dog     | f    | 1989-05-13 | NULL       |
  | Fang   | Benny  | dog     | m    | 1990-08-27 | NULL       |
  | Bowser | Diana  | dog     | m    | 1979-08-31 | 1995-07-29 |
  +--------+--------+---------+------+------------+------------+
  3 rows in set (0.00 sec)

  mysql> select * from pet where  (species = 'dog' or sex = 'm' ) and death is  NOT NULL;
  +--------+-------+---------+------+------------+------------+
  | name   | owner | species | sex  | birth      | death      |
  +--------+-------+---------+------+------------+------------+
  | Bowser | Diana | dog     | m    | 1979-08-31 | 1995-07-29 |
  +--------+-------+---------+------+------------+------------+
  1 row in set (0.00 sec)

IN Clause
------------

The IN operator allows a range of filter criteria to be specified in a WHERE clause, all contained in parentheses and comma separated. 


::

  mysql> select * from pet where sex IN ('f','m');
  +--------+--------+---------+------+------------+------------+
  | name   | owner  | species | sex  | birth      | death      |
  +--------+--------+---------+------+------------+------------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL       |
  | Claws  | Gwen   | cat     | m    | 1994-03-17 | NULL       |
  | Bluffy | Harold | dog     | f    | 1989-05-13 | NULL       |
  | Fang   | Benny  | dog     | m    | 1990-08-27 | NULL       |
  | Bowser | Diana  | dog     | m    | 1979-08-31 | 1995-07-29 |
  | Chirpy | Gwen   | bird    | f    | 1998-09-11 | NULL       |
  | Slim   | Benny  | snake   | m    | 1996-04-29 | NULL       |
  +--------+--------+---------+------+------------+------------+
  7 rows in set (0.00 sec)

Another way to write the statement is using the OR operator.

::

  mysql> select * from pet where sex = 'f' OR sex = 'm';
  +--------+--------+---------+------+------------+------------+
  | name   | owner  | species | sex  | birth      | death      |
  +--------+--------+---------+------+------------+------------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL       |
  | Claws  | Gwen   | cat     | m    | 1994-03-17 | NULL       |
  | Bluffy | Harold | dog     | f    | 1989-05-13 | NULL       |
  | Fang   | Benny  | dog     | m    | 1990-08-27 | NULL       |
  | Bowser | Diana  | dog     | m    | 1979-08-31 | 1995-07-29 |
  | Chirpy | Gwen   | bird    | f    | 1998-09-11 | NULL       |
  | Slim   | Benny  | snake   | m    | 1996-04-29 | NULL       |
  +--------+--------+---------+------+------------+------------+
  7 rows in set (0.00 sec)

NOT operator
--------------

The NOT operator is used to negate the result of an expression and is of particular use when using the IN operator.

::

  mysql> select * from pet where sex NOT IN ('f','m');
  +----------+-------+---------+------+------------+-------+
  | name     | owner | species | sex  | birth      | death |
  +----------+-------+---------+------+------------+-------+
  | Whistler | Gwen  | bird    | N    | 1997-12-09 | NULL  |
  +----------+-------+---------+------+------------+-------+
  1 row in set (0.00 sec)


Wildcard Filtering
--------------------

Wildcards are used when matching string values. A normal comparison requires the use of specific strings where each character in the two strings must match exactly before they are considered equal. Wildcards, on the other hand, provide more flexibility by allowing any character or group of characters in a string to be acceptable as a match for another string.

In the below example lets say I want to check for names which have word
'luffy' in the name.

::

  mysql> select name,owner,species,sex from pet where name IN ('fluffy','bluffy');
  +--------+--------+---------+------+
  | name   | owner  | species | sex  |
  +--------+--------+---------+------+
  | Fluffy | Harold | cat     | f    |
  | Bluffy | Harold | dog     | f    |
  +--------+--------+---------+------+
  2 rows in set (0.00 sec)

Using the LIKE operator we can achieve above using the followoing examples.

::

  mysql> select name,owner,species,sex from pet where name like '%luffy';
  +--------+--------+---------+------+
  | name   | owner  | species | sex  |
  +--------+--------+---------+------+
  | Fluffy | Harold | cat     | f    |
  | Bluffy | Harold | dog     | f    |
  +--------+--------+---------+------+
  2 rows in set (0.00 sec)


We can also achieve this using '_' with the LIKE operator.

::

  mysql> select name,owner,species,sex from pet where name like '_luffy';
  +--------+--------+---------+------+
  | name   | owner  | species | sex  |
  +--------+--------+---------+------+
  | Fluffy | Harold | cat     | f    |
  | Bluffy | Harold | dog     | f    |
  +--------+--------+---------+------+
  2 rows in set (0.00 sec)

To find owner whose names starts with 'H'

::

  mysql> select name,owner,species,sex from pet where owner like 'H%';
  +--------+--------+---------+------+
  | name   | owner  | species | sex  |
  +--------+--------+---------+------+
  | Fluffy | Harold | cat     | f    |
  | Bluffy | Harold | dog     | f    |
  +--------+--------+---------+------+
  2 rows in set (0.00 sec)

To find owner whose name ends with 'en'

::

  mysql> select * from pet where owner like '%en';
  +----------+-------+---------+------+------------+-------+
  | name     | owner | species | sex  | birth      | death |
  +----------+-------+---------+------+------------+-------+
  | Claws    | Gwen  | cat     | m    | 1994-03-17 | NULL  |
  | Chirpy   | Gwen  | bird    | f    | 1998-09-11 | NULL  |
  | Whistler | Gwen  | bird    | N    | 1997-12-09 | NULL  |
  +----------+-------+---------+------+------------+-------+
  3 rows in set (0.00 sec)

To find owner whose name contains 'e'

::

  mysql> select * from pet where owner like '%e%';
  +----------+-------+---------+------+------------+-------+
  | name     | owner | species | sex  | birth      | death |
  +----------+-------+---------+------+------------+-------+
  | Claws    | Gwen  | cat     | m    | 1994-03-17 | NULL  |
  | Fang     | Benny | dog     | m    | 1990-08-27 | NULL  |
  | Chirpy   | Gwen  | bird    | f    | 1998-09-11 | NULL  |
  | Whistler | Gwen  | bird    | N    | 1997-12-09 | NULL  |
  | Slim     | Benny | snake   | m    | 1996-04-29 | NULL  |
  +----------+-------+---------+------+------------+-------+
  5 rows in set (0.00 sec)

Find a species which has just three characters.

::

  mysql> select * from pet where species like '___';
  +--------+--------+---------+------+------------+------------+
  | name   | owner  | species | sex  | birth      | death      |
  +--------+--------+---------+------+------------+------------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL       |
  | Claws  | Gwen   | cat     | m    | 1994-03-17 | NULL       |
  | Bluffy | Harold | dog     | f    | 1989-05-13 | NULL       |
  | Fang   | Benny  | dog     | m    | 1990-08-27 | NULL       |
  | Bowser | Diana  | dog     | m    | 1979-08-31 | 1995-07-29 |
  +--------+--------+---------+------+------------+------------+
  5 rows in set (0.00 sec)

REGULAR EXPRESSIONS
--------------------

Regular expressions are essentially a set of instructions using a predefined syntax for matching text in a variety of flexible ways.Regular expressions are a feature common to many programming language.MySQL does not provide as extensive support for regular expressions as some other programming languages.


Regular expression character matching
--------------------------------------

To understand  the REGEXP operator, we will begin by looking at a use of regular expressions that could similarly be used with the LIKE operator.

::

  mysql> select name,owner,species,sex from pet where name regexp '.luffy';
  +--------+--------+---------+------+
  | name   | owner  | species | sex  |
  +--------+--------+---------+------+
  | Fluffy | Harold | cat     | f    |
  | Bluffy | Harold | dog     | f    |
  +--------+--------+---------+------+
  2 rows in set (0.01 sec)

Matching group of Characters
------------------------------

Regular expressions allow us to specify a group of acceptable character matches for any character position. The syntax for this requires that the characters be places in square brackets at the desired location in the match text.

::

  mysql> select name,owner,species,sex from pet where name regexp '[fb]luffy';
  +--------+--------+---------+------+
  | name   | owner  | species | sex  |
  +--------+--------+---------+------+
  | Fluffy | Harold | cat     | f    |
  | Bluffy | Harold | dog     | f    |
  +--------+--------+---------+------+
  2 rows in set (0.01 sec)

Matching from Range of characters
----------------------------------

The character group matching syntax can be extended to cover range of characters. For example, instead of declaring a regular expression to cover the letters between A and F as [ABCDEF] we could simply specify a range of characters using the '-' character between the upper and lower ranges [A-F].

::


  mysql> select * from pet where birth regexp  '19[1-9][1-9]-0[2-3]-[1-31]';
  +-------+-------+---------+------+------------+-------+
  | name  | owner | species | sex  | birth      | death |
  +-------+-------+---------+------+------------+-------+
  | Claws | Gwen  | cat     | m    | 1994-03-17 | NULL  |
  +-------+-------+---------+------+------------+-------+
  1 row in set (0.01 sec)

Handling Special Characters
-----------------------------

Regular expressions assign special meaning to particular characters. For instance the dot (.) and square brackets ([]) all have special meaning. If you are are looking for text that looks like a regular expression, the text for which you want to search is, itself, going to be viewed as regular expression syntax.To address thisissue, a concept known as escaping is used. In SQL, escaping involves preceding any characters that may be misinterpreted as a regular expression special character with double back slashes (\\).

To understand the example I have inserted the following values into the pet table.

::

  mysql> insert into pet values ('puppy','santosh','[unknown]','m',NULL,NULL);
  Query OK, 1 row affected (0.04 sec)

  mysql> select * from pet where owner='santosh';
  +-------+---------+-----------+------+-------+-------+
  | name  | owner   | species   | sex  | birth | death |
  +-------+---------+-----------+------+-------+-------+
  | puppy | santosh | [unknown] | m    | NULL  | NULL  |
  +-------+---------+-----------+------+-------+-------+
  1 row in set (0.00 sec)

Now to grab the species where its unknown.

  mysql> select * from pet where species='\[unknown\]';
  +-------+---------+-----------+------+-------+-------+
  | name  | owner   | species   | sex  | birth | death |
  +-------+---------+-----------+------+-------+-------+
  | puppy | santosh | [unknown] | m    | NULL  | NULL  |
  +-------+---------+-----------+------+-------+-------+
  1 row in set (0.00 sec)


Matching by character types.
----------------------------

One more way of matching  characters is by type or class.The character can be a letter,a number or alphanumber.


Class Keyword Description of Matches
[[:alnum:]] Alphanumeric - any number or letter. Equivalent to [a-z], [A-Z] and [0-9]
[[:alpha:]] Alpha - any letter. Equivalent to [a-z] and [A-Z]
[[:blank:]] Space or Tab. Equivalent to [\\t] and [ ]
[[:cntrl:]] ASCII Control Character
[[:digit:]] Numeric. Equivalent to [0-9]
[[:graph:]] Any character with the exception of space
[[:lower:]] Lower case letters. Equivalent to [a-z]
[[:print:]] Any printable character
[[:punct:]] Characters that are neither control characters, nor alphanumeric (i.e punctuation characters)
[[:space:]] Any whitespace character (tab, new line, form feed, space etc)
[[:upper:]] Upper case letters. Equivalent to [A-Z]
[[:xdigit:]]  Any hexadecimal digit. Equivalent to [A-F], [a-f] and [0-9] 

Lets now look at some examples.

::

  mysql> select * from pet where birth regexp '[[:alnum:]]-02-04';
  +--------+--------+---------+------+------------+-------+
  | name   | owner  | species | sex  | birth      | death |
  +--------+--------+---------+------+------------+-------+
  | Fluffy | Harold | cat     | f    | 1992-02-04 | NULL  |
  +--------+--------+---------+------+------------+-------+
  1 row in set (0.00 sec)

Repetition Metacharacters 
--------------------------

Regular expressions can also be written to look for repetition in text.


Metacharacter Description
* Any number of matches
+ One or more matches
{n} n number of matches
{n,}  Not less than n number of matches
{n1,n2} A range of matches between n1 and n2
? Optional single character match (character my be present or not to qualify for a match)

::

  mysql> select * from pet where name regexp '[[:alpha:]]{6}';
  +----------+--------+---------+------+------------+------------+
  | name     | owner  | species | sex  | birth      | death      |
  +----------+--------+---------+------+------------+------------+
  | Fluffy   | Harold | cat     | f    | 1992-02-04 | NULL       |
  | Bluffy   | Harold | dog     | f    | 1989-05-13 | NULL       |
  | Bowser   | Diana  | dog     | m    | 1979-08-31 | 1995-07-29 |
  | Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
  | Whistler | Gwen   | bird    | NULL | 1997-12-09 | NULL       |
  +----------+--------+---------+------+------------+------------+
  5 rows in set (0.00 sec)

Matching by Text Positions
---------------------------

The final area of regular expressions to cover in this chapter involves matching based on the location of text in a string. we may want to find a particular match that requires that a word appears at the beginning or end of a piece of text.


Metacharacter Description
^ Beginning of text
$ End of text
[[:<:]] Start of word
[[:>:]] End of word



Select the rows where species start with work 'b'.

::

  mysql> select * from pet where species regexp '^b[[:alpha:]]';
  +----------+-------+---------+------+------------+-------+
  | name     | owner | species | sex  | birth      | death |
  +----------+-------+---------+------+------------+-------+
  | Chirpy   | Gwen  | bird    | f    | 1998-09-11 | NULL  |
  | Whistler | Gwen  | bird    | NULL | 1997-12-09 | NULL  |
  +----------+-------+---------+------+------------+-------+
  2 rows in set (0.00 sec)

Select the rows where text ends with word rd.

::

  mysql> select * from pet where species regexp '[[:alpha:]]rd$';
  +----------+-------+---------+------+------------+-------+
  | name     | owner | species | sex  | birth      | death |
  +----------+-------+---------+------+------------+-------+
  | Chirpy   | Gwen  | bird    | f    | 1998-09-11 | NULL  |
  | Whistler | Gwen  | bird    | NULL | 1997-12-09 | NULL  |
  +----------+-------+---------+------+------------+-------+
  2 rows in set (0.00 sec)

Select the rows where text contains the word 'hi'

::

  mysql> select * from pet where name regexp 'hi';
  +----------+-------+---------+------+------------+-------+
  | name     | owner | species | sex  | birth      | death |
  +----------+-------+---------+------+------------+-------+
  | Chirpy   | Gwen  | bird    | f    | 1998-09-11 | NULL  |
  | Whistler | Gwen  | bird    | NULL | 1997-12-09 | NULL  |
  +----------+-------+---------+------+------------+-------+
  2 rows in set (0.00 sec)


