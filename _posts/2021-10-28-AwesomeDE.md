---
layout: post
author: ledinhtrunghieu
title: Awesome Data Engineering Learning path
---

A note for interesting I learned from this path

# 1. SQL

Querying data using SQL is an essential skill for anyone who works with data

## 1.1. Head First SQL: Your Brain on SQL - Lynn Beighley

A column is a piece of data stored by your table. A row is a single set of columns that describe attributes of a single thing. Columns ( or fields) and rows (or records) together make up a table.

Why do I need to create a database if I only have one table?
SQL is the ability to control access to your tables by multiple users. Being able to grant or deny access to an entire database is sometimes simpler than having to control the permissions on each one of multiple tables

```sql
doughnut_cost DEC(3,2) NOT NULL DEFAULT 1.00
```
Using a DEFAULT value fills the empty columns with a specified value.

```sql
INSERT INTO your_table (column_name1, column_name2,…)
VALUES ('value1', 'value2',… );
```

Add a backslash in front of the single quote
```sql
INSERT INTO my_contacts (location)
VALUES ('Grover\'s Mill');
```

```sql
WHERE location LIKE '%CA';
```
End with CA



# 2. Programming language

# 3. Relational Databases - Design & Architecture

# 4. noSql

# 5. Columnar Databases

# 6. Data warehouses

# 7. OLAP Data modeling

