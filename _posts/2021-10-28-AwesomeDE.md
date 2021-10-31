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

```sql
rating NOT IN ('innovative','fabulous', 'delightful','pretty good');

SELECT * FROM easy_drinks
WHERE NOT drink_name <> 'Blackthorn';

SELECT * FROM easy_drinks
WHERE NOT main = 'soda'
AND NOT main = 'iced tea';
```

```sql
UPDATE doughnut_ratings (table)
SET
type = 'glazed'
WHERE type = 'plain glazed'
```

**Atomic data**
RULE 1: A column with atomic data can't have several values of the same type of data in that column
RULE 2: A table with atomic data can't have multiple columns with the same type of data.

Normal tables won’t have duplicate data, which will reduce the size of your database

With less data to search through, your queries will be faster

```sql
CREATE TABLE my_contacts
(
contact_id INT NOT NULL AUTO_INCREMENT,
```

Table altering
```sql
ALTER TABLE
CHANGE both the name and data type of an existing column *
MODIFY the data type or position of an existing column *
ADD a column to your table—you pick the data type
DROP a column from your table *

ALTER TABLE projekts
RENAME TO project_list;
rename table

ALTER TABLE project_list
CHANGE COLUMN number proj_id INT NOT NULL AUTO_INCREMENT,
ADD PRIMARY KEY (proj_id);
```

To SELECT everything in front of the comma
```sql
SUBSTRING_INDEX()

Return a substring of a string before a specified number of delimiter occurs:

SELECT SUBSTRING_INDEX(location, ',', 1) FROM my_contacts;

This is the tricky part. It’s “1” because it’s looking for the first comma. If it were “2” it would keep going until it found a second comma and grab everything in front of that
```

```sql
LIMIT 0,4
```
LIMIT to position 0 3

A description of the data (the columns and tables) in your database, along with any other related objects and the way they all connect is known as a SCHEMA

CREATE a table with a FOREIGN KEY
```sql
CREATE TABLE interests (
int_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
interest VARCHAR(50) NOT NULL,
contact_id INT NOT NULL,
CONSTRAINT my_contacts_contact_id_fk
FOREIGN KEY (contact_id)
REFERENCES my_contacts (contact_id)
);
```

First Normal Form, or 1NF:
Rule 1: Columns contain only atomic values
Rule 2: No repeating groups of data

A COMPOSITE KEY is a PRIMARY KEY composed of multiple columns, creating a unique key

When a column’s data must change when another column’s data is modified, the first column is functionally dependent on the second
A dependent column is one containing data that could change if another column changes. Non-dependent columns stand alone.


A partial functional dependency means that a non-key column is dependent on some, but not all, of the columns in a composite primary key

If changing any of the non-key columns might cause any of the other columns to change, you have a transitive dependency


# 2. Programming language

# 3. Relational Databases - Design & Architecture

# 4. noSql

# 5. Columnar Databases

# 6. Data warehouses

# 7. OLAP Data modeling

