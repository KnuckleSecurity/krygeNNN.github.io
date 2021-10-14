---
title: Fundamentals of SQL Injection
author: krygennn
date: 2021-10-13 12:00
categories: [Blogging, cyber-security]
image:
    src: /assets/img/posts/fundamentals-of-sql-injection/sql2.jpg
    alt: SQL INJECTION
    width: 600
    height: 350
tags: [sql,sql-injection]
---
In this article you will learn SQL Injection fundamentals from 0 to hero.
<br>
In order to understand SQL injections, it is essential to observe those two concepts seperately.
## What is SQL ?
SQL stands for **simple query language**. What it does is basically, managing data held in databases.
It is particularly useful in handling structured data. So whenever you ask for spesific part of data
you can request it in any order or concatanate it with different data held in different data table. 

### Understanding SQL
Let us examine the behavior of SQL codes by using [**sqliteonline**](https://www.sqliteonline.com)

![Desktop View](/assets/img/posts/fundamentals-of-sql-injection/sql.jpg)
<br>

#### Why SQL ?
As you can see, there is a table consists of 3 columns and 21 rows.Those are basically key and value pairs.
In this spesific example `ID` is the **Key** value, `Name` and `Hint` are the values which defined by that 
spesific 'ID' numer of **1**.However, as you can guess, it is not practical to summon whole data each 
time when we want to use or see one or couple parts of the data. In addition, we may want to combine different 
data parts, so that is why we use SQL.

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
You can order the table with `ORDER BY` clause.You can either puth column number with numerics, or column name
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

