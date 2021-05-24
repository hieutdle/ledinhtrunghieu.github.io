---
layout: post
author: ledinhtrunghieu
title: Lesson 15 - Introduction to Relational Databases in SQL
---

# 1. Introduction to relational databases

**A relational database:**
* Real-life entities become tables.
* Reduced redundancy 
* Data integrity by relationships

```sql
SELECT table_schema, table_name 
FROM information_schema.tables;
```
<img src="/assets/images/20210506_IntroRDInSQL/pic1.png" class="largepic"/>


"information_schema" is actually some sort of meta-database that holds information about your current database. It's not PostgreSQL specific and also available in other database management systems like MySQL or SQL Server. This "information_schema" database holds various information in different tables, for example in the "tables" table.

```sql
SELECT table_name, column_name, data_type FROM information_schema.columns
WHERE table_name = 'pg_config';
```

"information_schema" also holds information about columns in the "columns" table. Once you know the name of a table, you can query its columns by accessing the "columns" table. Here, for example, you see that the system table "pg_config" has only two columns – supposedly for storing name-value pairs.

