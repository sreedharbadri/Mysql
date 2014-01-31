Today we talk about functions. Functions in SQL enable you to perform feats such as determining the sum of a column or converting all the characters of a string to uppercase.

We will be working on the following functions:

• Aggregate functions.
• Date and time functions.
• Arithmetic functions.
• Character functions.
• Miscellaneous functions.

Aggregate functions

Sometimes when working with the data stored in a MySQL database table, we are interested not in the data itself, but in statistics about that data. For example, whilst we may not be concerned about the specific content in each row, we may want to know how many rows are in a table. Alternatively, we may need to find the average of all the values in a particular table column.

Function  Description
Avg() Return  the average of the values in selected columns.
Count() Return the number of rows returned for selection.
Max() Return the maximum value for a column.
Min() Return the minimum value for a column.
Sum() Return the sum of values in a specified column.

For accomplishing the above said functions lets work on followoing examples.

mysql> desc employee;

+-------------+-------------+------+-----+---------+-------+

| Field       | Type        | Null | Key | Default | Extra |

+-------------+-------------+------+-----+---------+-------+

| id          | int(1)      | YES  |     | NULL    |       |

| name        | varchar(10) | YES  |     | NULL    |       |

| designation | varchar(10) | YES  |     | NULL    |       |

| salary      | int(4)      | YES  |     | NULL    |       |

+-------------+-------------+------+-----+---------+-------+

4 rows in set (0.01 sec)

mysql> select * from employee;

+------+---------+-------------+--------+

| id   | name    | designation | salary |

+------+---------+-------------+--------+

|    1 | santosh | admin       |   6000 |

|    2 | dinesh  | accountant  |   7000 |

|    3 | hari    | programmer  |   8000 |

|    4 | anu     | web admin   |   4000 |

|    6 | vijaya  | designer    |   5000 |

|    7 | karthik | engineer    |   8000 |

|    8 | chanu   | admin       |   9000 |

|    9 | leela   | engineer    |   3000 |

+------+---------+-------------+--------+

8 rows in set (0.00 sec)


COUNT

The 'COUNT' function is used to count all the rows of tables.

mysql> select count(*) from employee;

+----------+

| count(*) |

+----------+

|        8 |

+----------+

1 row in set (0.00 sec)


MIN

The 'MIN' function is used to find the minimum value of the specific field.

mysql> select name,designation,min(salary) from employee;

+---------+-------------+-------------+

| name    | designation | min(salary) |

+---------+-------------+-------------+

| santosh | admin       |        3000 |

+---------+-------------+-------------+

1 row in set (0.02 sec)


MAX

The 'MAX' function is used to find the maximum value of the specific field.

mysql> select name,designation,max(salary) from employee;

+---------+-------------+-------------+

| name    | designation | max(salary) |

+---------+-------------+-------------+

| santosh | admin       |        9000 |

+---------+-------------+-------------+

1 row in set (0.00 sec)

Now, lets try to use both MIN and MAX functions and find the maximum and minimum salaries of the employees GROUP BY designation.

mysql> select designation,min(salary),max(salary) from employee group by designation;

+-------------+-------------+-------------+

| designation | min(salary) | max(salary) |

+-------------+-------------+-------------+

| accountant  |        7000 |        7000 |

| admin       |        6000 |        9000 |

| designer    |        5000 |        5000 |

| engineer    |        3000 |        8000 |

| programmer  |        8000 |        8000 |

| web admin   |        4000 |        4000 |

+-------------+-------------+-------------+

6 rows in set (0.03 sec)


SUM

The 'SUM' function is used to sum all the values of the specific column of the table.

mysql> select sum(salary) as cost_to_company from employee;

+-----------------+

| cost_to_company |

+-----------------+

|           50000 |

+-----------------+

1 row in set (0.00 sec)

AVG

The 'AVG' function is used to find average of the specific column of the table.

mysql> select avg(salary) as average_salary from employee;

+----------------+

| average_salary |

+----------------+

|      6250.0000 |

+----------------+

1 row in set (0.00 sec)


Date and time functions

we will look into how MySQL handles time. We will understand the formats that MySQL uses to represent dates and time, and learn how to use the special functions MySQL has that help you handle date  and time formatting and arithmetic.

How dates are treated in Mysql

MySQL has a range of data types for handling date and time information. In general, MySQL will accept  several formats when being given data to put into a field. However, date and time output will always be  standardized and appear in a predictable format.



All date and time data types have a range of legal values and a "zero" value to which a field will be set if  you attempt to put an illegal value into it.

Time formats have an intuitive order that you're used to in daily life: hours at the left of the field,
minutes, and then seconds.

Dates, on the other hand, are always output with year at the left, month, and then day (never
 day/month/year).

When outputting date and time, you usually have the option to ask MySQL to give you the data as either  string or numeric data, even though it is the same information. The format used will depend on the  context in which it is used in your SQL.

MySQL is quite flexible when accepting date and time data: for example, the date 2001-05-12 means the same to MySQL as 2001/05/12, 1+5+12, or even 20010512. You can use a wide range of separators between the components of the field or none at all, and you can omit the leading zero for  numbers less than 10.

MySQL does partial checking of date and time information. For example, it expects days to be in the  range 1 to 31 and months in the range 1 to 12. However, it does not rigorously check whether a specific  date can really exist. It will not reject 30 February, for example. This makes it more efficient when  accepting data, putting the responsibility on your application to ensure valid dates are being entered.

Date related function

ADDDATE()        - add dates
ADDTIME()       - add time
convert_tz()      - convert from one timezone to another timezone.
curdate()    - return the current date.
current_date()
current_date
now()                   - return the current time.              
current_timestamp()
current_timestamp
curtime()
localtime()
localtime
localtimestamp
localtimestamp()
date()                  - extract the date part of a date or datatime expression.
second()                - return the seconds()
minute()                - return the minutes for the argument.
hour()                  - extract exact hour.
day()                   
week()                  - return the week number.
month()                 - return the month for the argument.
dayofweek()             - return the weekday index of the argument.
dayofmonth()            - return the day of month(1-31)
dayname()               - return name of the weekend.
monthname()             - return the month name
date_add()              - add two dates.
date_sub()              - substract two dates.
date_format()           - formate date as specified.
datediff()              - substract two days.
extract                 - extract part of the week.
from_days()             - convert a day number to date.
from_unixtime()         - format date as  UNIX timestamp.
last_day                - return the lastday of the month for the argument.
makedate()              - create a date from year and day of year.
maketime
microsecond()           - return the microsecond from argument.
period_add()            - add a period a year month.
period_diff()           - return the numbers of months from period.
quater()                - return the quater from a date argument.
sec_to_time()           - converts second to 'HH:MM:SS' format.
str_to_date()           - convert a string to date
subdate()               - when invoked with three arguments a synonym for date_sub()
subtime()               - substract time
sysdate()               - return at the time at which the function executes.
time_format()           - format a time
time_to_sec()           - return the arguments converted to seconds.
time()                  - extract the time portion of the expression passed.
timediff()              - substrace time
timestamp()             - With a single argument, this function returns             the date or datetime expression. With two               arguments, the sum of the arguments
timestampadd()          - add an interval to a datetime expression
timestampdiff()         - substract an interval from a datetime expression.
to_days()               - return the date arguments converted to days.
unix_timestamp()        - return a unix timestamp
utc_date()              - return the current UTC date.
utc_time()              - return the current UTC time.
weekday()               - return the weekday index.
weekofyear()            - return the calendar week of the date(1-53)
yearweek()              - return the year and week.
Date formates

%a      Abbreviated weekday name (Sun..Sat)
%b      Abbreviated month name (Jan..Dec)
%c      Month, numeric (0..12)
%D      Day of the month with English suffix (0th, 1st, 2nd, 3rd, .)
%d      Day of the month, numeric (00..31)
%e      Day of the month, numeric (0..31)
%f      Microseconds (000000..999999)
%H      Hour (00..23)
%h      Hour (01..12)
%I      Hour (01..12)
%i      Minutes, numeric (00..59)
%j      Day of year (001..366)
%k      Hour (0..23)
%l      Hour (1..12)
%M      Month name (January..December)
%m      Month, numeric (00..12)
%p      AM or PM
%r      Time, 12-hour (hh:mm:ss followed by AM or PM)
%S      Seconds (00..59)
%s      Seconds (00..59)
%T      Time, 24-hour (hh:mm:ss)
%U      Week (00..53), where Sunday is the first day of the week
%u      Week (00..53), where Monday is the first day of the week
%V      Week (01..53), where Sunday is the first day of the week; used     with %X
%v      Week (01..53), where Monday is the first day of the week; used         with %x
%W      Weekday name (Sunday..Saturday)
%w      Day of the week (0=Sunday..6=Saturday)
%X      Year for the week where Sunday is the first day of the week,numeric, four digits; used with %V
%x      Year for the week, where Monday is the first day of the week,      numeric, four digits; used with %v
%Y      Year, numeric, four digits
%y      Year, numeric (two digits)
%%      A literal .%. character
%x      x, for any.x. not listed above

For working on the above function lets take a table as example;

mysql> desc project;

+-----------+-------------+------+-----+---------+-------+

| Field     | Type        | Null | Key | Default | Extra |

+-----------+-------------+------+-----+---------+-------+

| task      | varchar(20) | YES  |     | NULL    |       |

| startdate | date        | YES  |     | NULL    |       |

| enddate   | date        | YES  |     | NULL    |       |

+-----------+-------------+------+-----+---------+-------+

3 rows in set (0.00 sec)


mysql> select * from project;

+---------------+------------+------------+

| task          | startdate  | enddate    |

+---------------+------------+------------+

| kickoff mtg   | 2013-04-01 | 2013-04-01 |

| tech survey   | 2013-04-02 | 2013-05-01 |

| user mtgs     | 2013-05-15 | 2013-05-30 |

| design widget | 2013-06-01 | 2013-06-30 |

| code widget   | 2013-07-01 | 2013-09-30 |

| testing       | 2013-09-03 | 2014-01-17 |

+---------------+------------+------------+

6 rows in set (0.00 sec)


NOW

mysql> select now();

+---------------------+

| now()               |

+---------------------+

| 2013-07-14 22:21:35 |

+---------------------+

1 row in set (0.00 sec)



MONTHNAME

What is current month name.

mysql> select monthname (now());

+-------------------+

| monthname (now()) |

+-------------------+

| July              |

+-------------------+

1 row in set (0.00 sec)

Arithmetic Functions

Many of the uses you have for the data you retrieve involve mathematics. Most  implementations of SQL provide arithmetic functions similar to the functions covered here.

mysql> desc number_game;

+-------+------------+------+-----+---------+-------+

| Field | Type       | Null | Key | Default | Extra |

+-------+------------+------+-----+---------+-------+

| a     | float(5,4) | YES  |     | NULL    |       |

| b     | float(5,4) | YES  |     | NULL    |       |

+-------+------------+------+-----+---------+-------+

2 rows in set (0.01 sec)



mysql> select * from number_game;

+---------+-------+

| a       | b     |

+---------+-------+

|  3.1415 |     4 |

|     -45 | 0.707 |

|       5 |     9 |

| -57.667 |    42 |

|      15 |    55 |

|    -7.2 |   5.3 |

+---------+-------+

6 rows in set (0.00 sec)


ABS

ABS returns the absolute value of the numbers.

mysql> select abs(a) as absolute from number_game;

+----------+

| absolute |

+----------+

|   3.1415 |

|  45.0000 |

|   5.0000 |

|  57.6670 |

|  15.0000 |

|   7.2000 |

+----------+

6 rows in set (0.00 sec)


CEIL

CEIL returns the smallest integer greater than or equal to its argument.

mysql> select b,ceil(b) from number_game;

+---------+---------+

| b       | ceil(b) |

+---------+---------+

|  4.0000 |       4 |

|  0.7070 |       1 |

|  9.0000 |       9 |

| 42.0000 |      42 |

| 55.0000 |      55 |

|  5.3000 |       6 |

+---------+---------+

6 rows in set (0.01 sec)



FLOOR

FLOOR does
 just the reverse, returning the largest integer equal to or less than its argument.

mysql> select b,floor(b) from number_game;

+---------+----------+

| b       | floor(b) |

+---------+----------+

|  4.0000 |        4 |

|  0.7070 |        0 |

|  9.0000 |        9 |

| 42.0000 |       42 |

| 55.0000 |       55 |

|  5.3000 |        5 |

+---------+----------+

6 rows in set (0.01 sec)


COS,COSH,SIN,SINH,TAN and TANH.

The COS, SIN, and TAN functions provide support for various trigonometric concepts. They all work on the assumption that n is in radians. The following statement returns  some unexpected values if you don't realize COS expects A to be in radians.

mysql> select a,cos(a) from number_game;

+----------+---------------------+

| a        | cos(a)              |

+----------+---------------------+

|   3.1415 | -0.9999999957073027 |

| -45.0000 |  0.5253219888177297 |

|   5.0000 | 0.28366218546322625 |

| -57.6670 |  0.4371831600175117 |

|  15.0000 | -0.7596879128588213 |

|  -7.2000 |  0.6083514659123751 |

+----------+---------------------+

6 rows in set (0.00 sec)




mysql> select a,sin(a) from number_game;

+----------+------------------------+

| a        | sin(a)                 |

+----------+------------------------+

|   3.1415 | 0.00009265740435792069 |

| -45.0000 |    -0.8509035245341184 |

|   5.0000 |    -0.9589242746631385 |

| -57.6670 |    -0.8993724949080346 |

|  15.0000 |     0.6502878401571168 |

|  -7.2000 |    -0.7936677478153338 |

+----------+------------------------+

6 rows in set (0.00 sec)



mysql> select a,tan(a) from number_game;

+----------+-------------------------+

| a        | tan(a)                  |

+----------+-------------------------+

|   3.1415 | -0.00009265740475567088 |

| -45.0000 |     -1.6197751905438615 |

|   5.0000 |      -3.380515006246586 |

| -57.6670 |      -2.057198394540196 |

|  15.0000 |     -0.8559934009085187 |

|  -7.2000 |     -1.3046204246833377 |

+----------+-------------------------+

6 rows in set (0.01 sec)


EXP

EXP enables you to raise e (e is a mathematical constant used in various formulas) to a
 power.

mysql> select a,exp(a) from number_game;

+----------+------------------------+

| a        | exp(a)                 |

+----------+------------------------+

|   3.1415 |     23.138548575594726 |

| -45.0000 | 2.8625185805493937e-20 |

|   5.0000 |      148.4131591025766 |

| -57.6670 |  9.026932429859777e-26 |

|  15.0000 |     3269017.3724721107 |

|  -7.2000 |  0.0007465859507766351 |

+----------+------------------------+

6 rows in set (0.00 sec)


LN

LN returns the natural logarithm of its
 argument.

mysql> select a,ln(a) from number_game;

+----------+--------------------+

| a        | ln(a)              |

+----------+--------------------+

|   3.1415 |  1.144700391646573 |

| -45.0000 |               NULL |

|   5.0000 | 1.6094379124341003 |

| -57.6670 |               NULL |

|  15.0000 |   2.70805020110221 |

|  -7.2000 |               NULL |

+----------+--------------------+

6 rows in set (0.00 sec)



If you see the negative values give NULL value.

mysql> select a,ln(abs(a)) from number_game;

+----------+--------------------+

| a        | ln(abs(a))         |

+----------+--------------------+

|   3.1415 |  1.144700391646573 |

| -45.0000 | 3.8066624897703196 |

|   5.0000 | 1.6094379124341003 |

| -57.6670 |  4.054685082984563 |

|  15.0000 |   2.70805020110221 |

|  -7.2000 |  1.974080999531056 |

+----------+--------------------+

6 rows in set (0.00 sec)



LOG

LOG, takes two arguments, returning the logarithm of the first argument in
 the base of the second.

mysql> select b,log(b,10) from number_game;

+---------+--------------------+

| b       | log(b,10)          |

+---------+--------------------+

|  4.0000 | 1.6609640474436813 |

|  0.7070 | -6.640962791038028 |

|  9.0000 | 1.0479516371446924 |

| 42.0000 | 0.6160483210529383 |

| 55.0000 | 0.5745928742534718 |

|  5.3000 | 1.3806893483446143 |

+---------+--------------------+

6 rows in set (0.00 sec)



MOD

Mod returns reminder of two numbers.

mysql> select mod(4,2);

+----------+

| mod(4,2) |

+----------+

|        0 |

+----------+

1 row in set (0.00 sec)


mysql> select a,b,mod(a,b) from number_game;

+----------+---------+----------+

| a        | b       | mod(a,b) |

+----------+---------+----------+

|   3.1415 |  4.0000 |   3.1415 |

| -45.0000 |  0.7070 |  -0.4590 |

|   5.0000 |  9.0000 |   5.0000 |

| -57.6670 | 42.0000 | -15.6670 |

|  15.0000 | 55.0000 |  15.0000 |

|  -7.2000 |  5.3000 |  -1.9000 |

+----------+---------+----------+

6 rows in set (0.00 sec)



SIGN

SIGN returns -1 if its argument is less than 0, 0 if its argument is equal to 0, and 1 if its
 argument is greater than 0.

mysql> select a,sign(a) from number_game;

+----------+---------+

| a        | sign(a) |

+----------+---------+

|   3.1415 |       1 |

| -45.0000 |      -1 |

|   5.0000 |       1 |

| -57.6670 |      -1 |

|  15.0000 |       1 |

|  -7.2000 |      -1 |

+----------+---------+

6 rows in set (0.00 sec)


SQRT

The function SQRT returns the square root of an argument. Because the square root of
 a negative number is undefined, you cannot use SQRT on negative numbers.

mysql> select a,sqrt(a) from number_game;

+----------+--------------------+

| a        | sqrt(a)            |

+----------+--------------------+

|   3.1415 | 1.7724277125415588 |

| -45.0000 |               NULL |

|   5.0000 |   2.23606797749979 |

| -57.6670 |               NULL |

|  15.0000 |  3.872983346207417 |

|  -7.2000 |               NULL |

+----------+--------------------+

6 rows in set (0.01 sec)



mysql> select abs(a),sqrt(abs(a)) from number_game;

+---------+--------------------+

| abs(a)  | sqrt(abs(a))       |

+---------+--------------------+

|  3.1415 | 1.7724277125415588 |

| 45.0000 |  6.708203932499369 |

|  5.0000 |   2.23606797749979 |

| 57.6670 |  7.593879102072572 |

| 15.0000 |  3.872983346207417 |

|  7.2000 |  2.683281537458404 |

+---------+--------------------+

6 rows in set (0.00 sec)




Charater Function

Many implementations of SQL provide functions to manipulate characters and strings of characters.
Lets use the following table as example.

mysql> desc chrt;

+-----------+-------------+------+-----+---------+-------+

| Field     | Type        | Null | Key | Default | Extra |

+-----------+-------------+------+-----+---------+-------+

| lastname  | varchar(10) | YES  |     | NULL    |       |

| firstname | varchar(10) | YES  |     | NULL    |       |

| alpha     | char(1)     | YES  |     | NULL    |       |

| code      | char(1)     | YES  |     | NULL    |       |

+-----------+-------------+------+-----+---------+-------+

4 rows in set (0.01 sec)





Lets populate the table with names of people

mysql> select * from chrt;

+----------+-----------+-------+------+

| lastname | firstname | alpha | code |

+----------+-----------+-------+------+

| dabberu  | santosh   | A     | 32   |

| yadav    | abhilasha | j     | 67   |

| goud     | sirisha   | c     | 65   |

| yvs      | kishore   | m     | 87   |

| cherry   | mounika   | a     | 77   |

| nagadev  | kalyan    | g     | 52   |

+----------+-----------+-------+------+

6 rows in set (0.00 sec)



CHAR

CHAR returns the character equivalent of the number it uses as an argument. The character it returns depends on the character set of the database. For this example the database is set to ASCII.

mysql> select code,char(code) from chrt;

+------+------------+

| code | char(code) |

+------+------------+

| 32   |            |

| 67   | C          |

| 65   | A          |

| 87   | W          |

| 77   | M          |

| 52   | 4          |

+------+------------+

6 rows in set (0.00 sec)


Note: 32 represents a space in ASCII character set.


CONCAT

Its a full form for concatination.

mysql> select concat(lastname,"  ", firstname) as Fullname from chrt;

+------------------+

| Fullname         |

+------------------+

| dabberu  santosh |

| yadav  abhilasha |

| goud  sirisha    |

| yvs  kishore     |

| cherry  mounika  |

| nagadev  kalyan  |

+------------------+

6 rows in set (0.00 sec)


UPPER and LOWER

LOWER changes all the characters to lowercase; UPPER does just the reverse.

mysql> SELECT UPPER('Hej');

        -> 'HEJ'


mysql> SELECT LOWER('QUADRATICALLY');

        -> 'quadratically'



Now lets work a bit on our table ,

mysql> select lastname,upper(lastname),lower(upper(lastname)) from chrt;

+----------+-----------------+------------------------+

| lastname | upper(lastname) | lower(upper(lastname)) |

+----------+-----------------+------------------------+

| dabberu  | DABBERU         | dabberu                |

| yadav    | YADAV           | yadav                  |

| goud     | GOUD            | goud                   |

| yvs      | YVS             | yvs                    |

| cherry   | CHERRY          | cherry                 |

| nagadev  | NAGADEV         | nagadev                |

+----------+-----------------+------------------------+

6 rows in set (0.00 sec)



LPAD and RPAD

LPAD and RPAD take a minimum of two and a maximum of three arguments. The first  argument is the character string to be operated on. The second is the number of characters to pad it with, and the optional third argument is the character to pad it with. The third argument defaults to a blank, or it can be a single character or a character string. 

Few examples:

mysql> SELECT LPAD('hi',4,'??');

        -> '??hi'

mysql> SELECT LPAD('hi',1,'??');

        -> 'h'


mysql> SELECT RPAD('hi',5,'?');

        -> 'hi???'

mysql> SELECT RPAD('hi',1,'?');

        -> 'h'


Lets get back to our tables.

mysql> select firstname,lpad(firstname,20,'*') from chrt;

+-----------+------------------------+

| firstname | lpad(firstname,20,'*') |

+-----------+------------------------+

| santosh   | *************santosh   |

| abhilasha | ***********abhilasha   |

| sirisha   | *************sirisha   |

| kishore   | *************kishore   |

| mounika   | *************mounika   |

| kalyan    | **************kalyan   |

+-----------+------------------------+

6 rows in set (0.00 sec)



Now lets try to give space between the firstname and lastname.

mysql> select concat(firstname,lpad(lastname,8,' ')) as fullname from chrt;

+-------------------+

| fullname          |

+-------------------+

| santosh dabberu   |

| abhilasha   yadav |

| sirisha    goud   |

| kishore     yvs   |

| mounika  cherry   |

| kalyan nagadev    |

+-------------------+

6 rows in set (0.00 sec)



Now an example on rpad,

mysql> select firstname,rpad(lastname,11,'*') from chrt;

+-----------+-----------------------+

| firstname | rpad(lastname,11,'*') |

+-----------+-----------------------+

| santosh   | dabberu****           |

| abhilasha | yadav******           |

| sirisha   | goud*******           |

| kishore   | yvs********           |

| mounika   | cherry*****           |

| kalyan    | nagadev****           |

+-----------+-----------------------+

6 rows in set (0.00 sec)


LTRIM and RTRIM

LTRIM and RTRIM take at least one and at most two arguments. The first argument, like
 LPAD and RPAD, is a character string. The optional second element is either a character
 or character string or defaults to a blank. If you use a second argument that is not a
 blank, these trim functions will trim that character the same way they trim the blanks.

mysql> select "  hello",ltrim("   hello") ;

+---------+-------------------+

| hello   | ltrim("   hello") |

+---------+-------------------+

|   hello | hello             |

+---------+-------------------+

1 row in set (0.00 sec)






mysql> select "  hello",rtrim("hello   ") ;

+---------+-------------------+

| hello   | rtrim("hello   ") |

+---------+-------------------+

|   hello | hello             |

+---------+-------------------+

1 row in set (0.00 sec)


REPLACE

Of its three arguments, the first is the string to be searched.The second is the search key. The last is the optional replacement string. If the third argument is left out or NULL, each occurrence of the search key on the string to be searched is removed and is not replaced with anything.

mysql> select lastname,replace(lastname, 'a', ' ' ) replacement from chrt;

+----------+-------------+

| lastname | replacement |

+----------+-------------+

| dabberu  | d bberu     |

| yadav    | y d v       |

| goud     | goud        |

| yvs      | yvs         |

| cherry   | cherry      |

| nagadev  | n g dev     |

+----------+-------------+

6 rows in set (0.00 sec)


mysql> select lastname,replace(lastname, 'a', 'A' ) replacement from chrt;

+----------+-------------+

| lastname | replacement |

+----------+-------------+

| dabberu  | dAbberu     |

| yadav    | yAdAv       |

| goud     | goud        |

| yvs      | yvs         |

| cherry   | cherry      |

| nagadev  | nAgAdev     |

+----------+-------------+

6 rows in set (0.00 sec)



mysql> select lastname,replace(lastname, 'a', NULL) replacement from chrt;

+----------+-------------+

| lastname | replacement |

+----------+-------------+

| dabberu  | NULL        |

| yadav    | NULL        |

| goud     | goud        |

| yvs      | yvs         |

| cherry   | cherry      |

| nagadev  | NULL        |

+----------+-------------+

6 rows in set (0.00 sec)


SUBSTR

This three-argument function enables you to take a piece out of a target string. The first argument is the target string. The second argument is the position of the first character to be output. The third argument is the number of characters to show.

mysql> select firstname,substr(firstname,2) from chrt;

+-----------+---------------------+

| firstname | substr(firstname,2) |

+-----------+---------------------+

| santosh   | antosh              |

| abhilasha | bhilasha            |

| sirisha   | irisha              |

| kishore   | ishore              |

| mounika   | ounika              |

| kalyan    | alyan               |

+-----------+---------------------+

6 rows in set (0.01 sec)


mysql> select firstname,substr(firstname,2,3) from chrt;

+-----------+-----------------------+

| firstname | substr(firstname,2,3) |

+-----------+-----------------------+

| santosh   | ant                   |

| abhilasha | bhi                   |

| sirisha   | iri                   |

| kishore   | ish                   |

| mounika   | oun                   |

| kalyan    | aly                   |

+-----------+-----------------------+

6 rows in set (0.01 sec)



mysql> select firstname,substr(firstname,-2) from chrt;

+-----------+----------------------+

| firstname | substr(firstname,-2) |

+-----------+----------------------+

| santosh   | sh                   |

| abhilasha | ha                   |

| sirisha   | ha                   |

| kishore   | re                   |

| mounika   | ka                   |

| kalyan    | an                   |

+-----------+----------------------+

6 rows in set (0.00 sec)



INSTR

To find out where in a string a particular pattern occurs, use INSTR. Its first argument
 is the target string. The second argument is the pattern to match. The third and forth
 are numbers representing where to start looking and which match to report.


mysql> select lastname,instr(lastname, 'a') from chrt;

+----------+----------------------+

| lastname | instr(lastname, 'a') |

+----------+----------------------+

| dabberu  |                    2 |

| yadav    |                    2 |

| goud     |                    0 |

| yvs      |                    0 |

| cherry   |                    0 |

| nagadev  |                    2 |

+----------+----------------------+

6 rows in set (0.00 sec)


LENGTH

LENGTH returns the length of its lone character argument.

mysql> select lastname,length(lastname) from chrt;

+----------+------------------+

| lastname | length(lastname) |

+----------+------------------+

| dabberu  |                7 |

| yadav    |                5 |

| goud     |                4 |

| yvs      |                3 |

| cherry   |                6 |

| nagadev  |                7 |

+----------+------------------+

6 rows in set (0.00 sec)


Miscellaneous functions

GREATEST and LEAST

With two or more arguments, returns the largest (maximum-valued) argument. The arguments are compared using the same rules as for LEAST().

mysql> SELECT GREATEST(2,0);

        -> 2

mysql> SELECT GREATEST(34.0,3.0,5.0,767.0);

        -> 767.0

mysql> SELECT GREATEST('B','A','C');

        -> 'C'


With two or more arguments, returns the smallest (minimum-valued)

argument


mysql> SELECT LEAST(2,0);

        -> 0

mysql> SELECT LEAST(34.0,3.0,5.0,767.0);

        -> 3.0

mysql> SELECT LEAST('B','A','C');

        -> 'A'















