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
