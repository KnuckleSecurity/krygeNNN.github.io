---
title: Fundamentals of SQL Injection
author: krygennn
date: 2021-10-13 12:00
categories: [Blogging, cyber-security]
image:
    src: /assets/img/posts/fundamentals-of-sql-injection/sql2.jpg
    alt: SQL INJECTION
    width: 800
    height: 450
tags: [sql,sql-injection]
---
In this article you will learn SQL Injection fundamentals from 0 to hero.
<br>
In order to understand SQL injections, it is essential to observe those two concepts seperately.

## WHAT IS INJECTION ?
Injection refers to a broad variety of attack vectors. An attacker feeds the program with malicious input.
This input interpreted by the program and works for attacker's favour.This vulnerability occurs when the input is not controlled 
with meticulousness before or after input has taken.There are bunch of different kind of injections out there.
Examples are javascript ,ldap, xpath, host header, code, email header and crlf injections and SQL injection is one of them.
## WHAT IS SQL ?
SQL stands for **simple query language**. What it does is basically, managing data held in databases.
It is particularly useful in handling structured data. So whenever you ask for spesific part of data
you can request it in any order or concatanate it with different data held in different data table. 
There are bunch of different sql types, MySQL, SqLite, PostgreSQL, Microsoft SQL Server and etc. They are
actually more or less all the same except for the syntax diffrences between them.

### Understanding SQL
Let us examine the behavior of SQL codes by using [**sqliteonline**](https://www.sqliteonline.com)

![Desktop View](/assets/img/posts/fundamentals-of-sql-injection/sql.jpg)
<br>

#### Why SQL ?
As you can see, there is a table consists of 3 columns and 21 rows.Those are basically **key** and **value** pairs.
In this spesific example `ID` is the **Key** value, `Name` and `Hint` are the **value**s which defined by that 
spesific 'ID' numer of **1**.However, as you can guess, it is not practical to summon whole data each 
time when we want to use or see one or couple parts of the data. In addition, we may want to combine different 
data parts together, so that is why we use SQL.

#### SQL Basics
Every table has its own name, and the name of the table above is **demo**.It is not needed to be an expert
SQL programmer, but you need to understand the principles of SQL programming, so let us start with some basics.

```sql
SELECT * FROM demo;
```
This command selects everything from demo table, fetches full content from the table.
You can see the output at the picture above. 
<br><br>
```sql
SELECT * FROM demo WHERE ID=3;
```
If you want to bring the informations related to 'ID' number of 3, you can add `WHERE` clause at the end of the query, and specify the filter.
![Desktop View](/assets/img/posts/fundamentals-of-sql-injection/sql3.jpg)
<br><br>
```sql
SELECT * FROM demo WHERE ID=3 UNION SELECT * FROM demo WHERE Name="Chart";
```
You can concatenate two different queries together by using 'UNION', however result of the
queries have to have same amount of columns, otherwise it will not work. In this example, there is
only one table called **demo**, so naturally all the selections will have 3 columns.If the result of the select queries of different
tables have different amount of columns, you can not `UNION` the result of the selections together.
![Desktop View](/assets/img/posts/fundamentals-of-sql-injection/sql4.jpg)
<br>
<br>
```sql
SELECT * FROM demo ORDER BY name ASC;
```
You can order the table with `ORDER BY` clause.You can either put column number with numerics, or column name
both `ASC` ascending and `DESC` descending order.
![Desktop View](/assets/img/posts/fundamentals-of-sql-injection/sql5.jpg)
<br><br>
```sql
SELECT table_name FROM INFORMATION_SCHEMA.TABLES ORDER BY 1 ASC;
```
See all the tables existing in the currently active database.
![Desktop View](/assets/img/posts/fundamentals-of-sql-injection/sql6.jpg)
<br><br>
```sql
SHOW DATABASES;
```
See all the created databases.
![Desktop View](/assets/img/posts/fundamentals-of-sql-injection/sql7.jpg)

##### Summary
Understanding this much of SQL will do for now.In this section I wrote about basics of SQL, however in the following
sections I will be writing about tricks and tactics about SQL Injection, it is more imporant to understand very fundamentals
rather than complex queries.

## UNDERSTANDING PRIMITIVE DATABASE LOGIC
From now on, you know how sql works, and the idea behind that. In this section, before moving on to the exploitation phase, 
you will learn bare-bones of **SQLi**, database logic and do some brainstorm. In this section I will be using **MariaDB**, it is community
developed database management system for MySQL.

### MariaDB/MySQL
I fired up my terminal and run my mariadb service. Without selecting any database or any table, I will run the following query.
Try to guess the result of it. 
```sql
MariaDB [(none)]> SELECT 1;
```
It returns
```
+---+
| 1 | >>> This is the column name
+---+
| 1 | >>> This is the value corresponds to that column.
+---+
```
Does not make too much sense right ? Let us move further.
<br>
```sql
MariaDB [(none)]> SELECT 2-1;
```
How do you think this will end up ?
```sql
+-----+
| 2-1 |
+-----+
|   1 |
+-----+
```
From now on, we know that we can do **mathematical operations** with integers in sql this is huge.
<br>
```sql
MariaDB [(none)]> SELECT '2-1';
```
It is same as before except for the single quotes.
```sql
+-----+
| 2-1 |
+-----+
| 2-1 |
+-----+
```
This time what we see is returning of a string value. It is reasonable because single or double quotes
are used for strings.
<br>
Everything was fine until this moment, however this time things going to change.
```sql
MariaDB [(none)]> SELECT '2'-'1';
```
It is hard to predict what is going to happen this time right ? Let us see.
```sql
+---------+
| '2'-'1' |
+---------+
|       1 |
+---------+
``` 
Database actually performed the same mathematical operation as before when we did `SELECT 2-1`. That is because just like you,
database's mind get confused too, so it came up with an idea that 'Oh wait, I can cast them into integers !'.Now we are one more
step closer to understand the primitive behaviour of how databases work actually.

```sql
MariaDB [(none)]> SELECT '2'+'a';
+---------+
| '2'+'a' |
+---------+
|       2 |
+---------+
```
Hmm, brains burning right ? It is simple really, just like before database converted string value to integer, however it could not convert
**a** to any integer, so it considered it as **0**.
<br>
By having this knowledge you should guess the output of this query.
```sql
MariaDB [(none)]> SELECT 'b'+'a';
```
You predicted it ?
```sql
+---------+
| 'b'+'a' |
+---------+
|       0 |
+---------+
```
This time two non-convertible to integer values given into single quoutes, database performed **0+0** this time.
