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

**First Normal Form, or 1NF:**
Rule 1: Columns contain only atomic values
Rule 2: No repeating groups of data

<img src="/assets/images/20211028_AwesomeDE/pic3.png" class="largepic"/>


A COMPOSITE KEY is a PRIMARY KEY composed of multiple columns, creating a unique key

<img src="/assets/images/20211028_AwesomeDE/pic1.png" class="largepic"/>

<img src="/assets/images/20211028_AwesomeDE/pic4.png" class="largepic"/>

When a column’s data must change when another column’s data is modified, the first column is functionally dependent on the second

A dependent column is one containing data that could change if another column changes. Non-dependent columns stand alone.

A partial functional dependency means that a non-key column is dependent on some, but not all, of the columns in a composite primary key

<img src="/assets/images/20211028_AwesomeDE/pic2.png" class="largepic"/>

<img src="/assets/images/20211028_AwesomeDE/pic5.png" class="largepic"/>


If changing any of the non-key columns might cause any of the other columns to change, you have a transitive dependency

**Cartesian join**
```sql
SELECT t.toy, b.boy
FROM toys AS t
CROSS JOIN
boys AS b;
```

An INNER JOIN combines the records from two tables using comparison operators in a condition. Columns are returned only where the joined rows match the condition. Let’s take a closer look at the syntax.

NATURAL JOIN inner joins identify matching column names.
```sql
ELECT boys.boy, toys.toy
FROM boys
NATURAL JOIN
toys;
```
A LEFT OUTER JOIN takes all the rows in the left table and matches them to rows in the RIGHT table. It’s useful when the left table and the right table have a one-to-many relationship

The difference is that an outer join gives you a row whether there’s a match with the other table or not.
A NULL value in the results of a left outer join means that the right table has no values that correspond to the left table.

<img src="/assets/images/20211028_AwesomeDE/pic6.png" class="largepic"/>

<img src="/assets/images/20211028_AwesomeDE/pic7.png" class="largepic"/>

**Self-join**
<img src="/assets/images/20211028_AwesomeDE/pic8.png" class="largepic"/>

```sql
SELECT c1.name, c2.name AS boss
FROM clown_info c1
INNER JOIN clown_info c2
ON c1.boss_id = c2.id
```

**Union rule**
<img src="/assets/images/20211028_AwesomeDE/pic9.png" class="largepic"/>

**Union All return all include duplicated like clown clown**
<img src="/assets/images/20211028_AwesomeDE/pic10.png" class="largepic"/>

**Creating a view**
```sql
CREATE VIEW web_designers AS
SELECT mc.first_name, mc.last_name, mc.phone, mc.email
FROM my_contacts mc
NATURAL JOIN job_desired jd
WHERE jd.title = 'Web Designer';
```

```sql
CREATE VIEW tech_writer_jobs AS
SELECT title, salary, description, zip
FROM job_listings
WHERE title = 'Technical Writer';
```
A VIEW is basically a table that only exists when you use the view in a query. It’s considered a virtual table because it acts like a table, and the same operations that can be performed on a table can be performed on a view. But the virtual table doesn’t stay in the database. It gets created when we use the view and then deleted. The named VIEW is the only thing that persists. This is good, because each time new rows are inserted into the database, when you use a view it will see the new information.

**Why views are good for your database**
* You can keep changes to your database structure from breaking applications that depend on your tables.
* Views make your life easier by simplifying your complex query into a simple command.
* You can create views that hide information that isn’t needed by the user.

**A transaction is a set of SQL statements that accomplish a single unit of work.**

During a transaction, if all the steps can’t be completed without interference, none of them should be completed.
<img src="/assets/images/20211028_AwesomeDE/pic11.png" class="largepic"/>

<img src="/assets/images/20211028_AwesomeDE/pic12.png" class="largepic"/>

<img src="/assets/images/20211028_AwesomeDE/pic13.png" class="largepic"/>

```sql
When a WHERE is done on an unindexed column, the RDBMS starts from the beginning
of that column and works its way through, one row at a time. If your table is huge, and
we mean 4 million rows huge, that can begin to take perceptible time.
When you create an index on a column, your RDBMS keeps additional information
about the column that speeds that searching up tremendously. The additional information
is kept in a behind-the-scenes table that is in a specific order the RDBMS can search
through more quickly. The trade-off is that indexes take up disk space. So you have to
consider creating some columns as indexes, the ones you’ll search on frequently, and not
indexing others.

In MySQL, we can use an ALTER statement to add an index called all_contacts_names:
ALTER TABLE all_contacts
ADD INDEX all_contacts_names(last_name, first_name);
We can also index those columns like this:
CREATE INDEX all_contacts_names
ON all_contacts (last_name, first_name);
An interesting effect of the all_contacts_names index is that when you perform a
query on the original table (e.g., SELECT * FROM all_contacts) the rows will be
alphabetically ordered by last_name and sub-ordered by first_name without you
specifying an order
```

## 1.2. Designing Data-Intensive Applications - Martin Kleppmann

Column Compression
Besides only loading those columns from disk that are required for a query, we can
further reduce the demands on disk throughput by compressing data. Fortunately,
column-oriented storage often lends itself very well to compression.

# 2. Programming language

# 3. Relational Databases - Design & Architecture

## 3.1. Normalization of Database - studytonight.com
Normalization is used for mainly two purposes,
* Eliminating redundant(useless) data.
* Ensuring data dependencies make sense i.e data is logically stored.

2-NF
<img src="/assets/images/20211028_AwesomeDE/pic14.png" class="largepic"/>

AXY : Candidate key, B depends on A which is a part of candidate key, not AXY => Paritial dependency
A -> B , if A and B both non-prime attribute => transitive dependency

Boyce-Codd Normal Form (BCNF)
It should be in the Third Normal Form.
And, for any dependency A → B, A should be a super key

prime -> non-prime : functional dependency
part of prime -> non-prime: partial
non -> non : transitive
non -> prime: not in BCNF
<img src="/assets/images/20211028_AwesomeDE/pic15.png" class="largepic"/>
<img src="/assets/images/20211028_AwesomeDE/pic16.png" class="largepic"/>

## 3.2. DBMS Architecture: 1-Tier, 2-Tier & 3-Tier - Guru99

A Database Architecture is a representation of DBMS design. It helps to design, develop, implement, and maintain the database management system. A DBMS architecture allows dividing the database system into individual components that can be independently modified, changed, replaced, and altered. It also helps to understand the components of a database.

**1-Tier Architecture**
1 Tier Architecture in DBMS is the simplest architecture of Database in which the client, server, and Database all reside on the same machine. A simple one tier architecture example would be anytime you install a Database in your system and access it to practice SQL queries. But such architecture is rarely used in production.

**2-Tier Architecture**
A 2 Tier Architecture in DBMS is a Database architecture where the presentation layer runs on a client (PC, Mobile, Tablet, etc.), and data is stored on a server called the second tier. Two tier architecture provides added security to the DBMS as it is not exposed to the end-user directly. It also provides direct and faster communication.

**3-Tier Architecture**
A 3 Tier Architecture in DBMS is the most popular client server architecture in DBMS in which the development and maintenance of functional processes, logic, data access, data storage, and user interface is done independently as separate modules. Three Tier architecture contains a presentation layer, an application layer, and a database server.

3-Tier database Architecture design is an extension of the 2-tier client-server architecture. A 3-tier architecture has the following layers:

1. Presentation layer (your PC, Tablet, Mobile, etc.)
2. Application layer (server)
3. Database Server

The goal of Three Tier client-server architecture is:
* To separate the user applications and physical database
* To support DBMS characteristics
* Program-data independence
* Supporting multiple views of the data
# 4. noSql

# 5. Columnar Databases

## 5.1. Data Processing Holy Grail? Row vs. Columnar Databases - Joao Sousa
OLAP vs. OLTP
OLTP, Online Transaction Processing, is the most traditional processing system. It is able to manage transaction-oriented applications and can be characterized by a large number of short, atomic database operations, such as inserts, updates, and deletes, that are quite common in your day-to-day application. Common examples include online banking and e-commerce applications.

OLAP, Online Analytical Processing, manages historical or archival data. It is characterized by a relatively low volume of transactions. OLAP systems are typically used for analytical purposes — to extract insights and knowledge from bulk data, merged from multiple sources. Unlike OLTP, the goal for OLAP systems is to have a limited amount of transactions, each consisting of mostly bulk reads and writes. Data warehouses are the typical infrastructure to maintain these systems.

Row-oriented databases are commonly used in OLTP systems, whereas columnar ones are more suitable for OLAP. 

**But What Makes Them Different Internally?**

The key difference between both data stores is the way they are physically stored on disk. Common HDDs are organized in blocks and rely on expensive head seek operations. Thus, sequential reads and writes tend to be much faster than random accesses.

Row-oriented databases store the whole row in the same block, if possible. Columnar databases store columns in subsequent blocks.
<img src="/assets/images/20211028_AwesomeDE/pic17.png" class="largepic"/>
<img src="/assets/images/20211028_AwesomeDE/pic18.png" class="largepic"/>

**Performance Example**
For simplification purposes, consider that the database administrator is a rookie and hasn’t implemented any indexes, partitioning, or any other optimization process on the database. With this in mind, for the analytical query:
What is the average age of males?
Row wise DB -> Has to read all the data in the database — 100GB to read.
Columnar DB -> Has to read only the columns age and gender — 2GB to read.

**Storage Size**
As you might recall from our first comparison table, columnar storage has improved compression mechanisms over row-wise databases. This happens essentially because each column, compressed as a single file, has a unique data type. Besides, as mentioned in the previous section, these databases don’t have indexes besides the one created by default for the (primary) key. 
Columnar databases are rather incredible at what they do — processing massive amounts of data in a matter of seconds or even less. There are many examples out there — Redshift, BigQuery, Snowflake, MonetDB, Greenplum, MemSQL, ClickHouse — and all offer optimal performance for your analytical needs.

However, their underlying architecture introduces a considerable caveat: data ingestion. They offer poor performance for mixed workloads, that require real-time high-throughput. In other words, they can’t match a transactional database’s real-time data ingestion performance, which allows the insertion of data into the system quite fast.

## 5.2. Why Column Stores? - John Schulz

Traditional databases are usually designed to query specific data from the database quickly and support transactional operations. While the capability is there to perform statistical aggregates, they are not usually very fast. Especially when you are working with databases containing hundreds of millions of rows.

To support purely analytic access to a database the column store was invented.

Column stores are lousy for transactional operations or selecting the contents of a specific row or record, but they can be awesome at producing statistics on a specific column in a table.

Row
<img src="/assets/images/20211028_AwesomeDE/pic19.png" class="largepic"/>

Column
<img src="/assets/images/20211028_AwesomeDE/pic20.png" class="largepic"/>

If you are going to store data in columns rather than rows the cost of doing single inserts is very high and the number of raw bytes is much larger than what you would normally store. As a result, most column stores are traditionally loaded in batches that can optimize the column store process and the data is pretty much always compressed on disk to overcome the increased amount of data to be stored. In addition to compression most modern column stores store column data in blocks with several precomputed statistics which will make queries run much faster allowing computation by accessing much less data.

**Parquet**
Parquet isn’t a database. Instead it’s a file format which can be used to store database tables on distributed file systems like HDFS, CEPH or AWS S3. Data is stored in Parquet in chunks that contain blocks of column data in a fashion that makes it possible to break up a Parquet file. Storing the file on many distributed hosts while allowing it to be processed in parallel. You can access Parquet files using Apache Spark, Hive, Pig, Apache Drill and Cloudera’s Impala.

# 6. Data warehouses

# 6.1. What is Data Warehouse? Types, Definition & Example - Guru99
A Data Warehousing (DW) is process for collecting and managing data from varied sources to provide meaningful business insights. A Data warehouse is typically used to connect and analyze business data from heterogeneous sources. The data warehouse is the core of the BI system which is built for data analysis and reporting.

It is a blend of technologies and components which aids the strategic use of data. It is electronic storage of a large amount of information by a business which is designed for query and analysis instead of transaction processing. It is a process of transforming data into information and making it available to users in a timely manner to make a difference.
The decision support database (Data Warehouse) is maintained separately from the organization’s operational database. However, the data warehouse is not a product but an environment. It is an architectural construct of an information system which provides users with current and historical decision support information which is difficult to access or present in the traditional operational data store.

You many know that a 3NF-designed database for an inventory system many have tables related to each other. For example, a report on current inventory information can include more than 12 joined conditions. This can quickly slow down the response time of the query and report. A data warehouse provides a new design which can help to reduce the response time and helps to enhance the performance of queries for reports and analytics.


# 7. OLAP Data modeling

# 8. Batch data processing & MapReduce
MapReduce is a software framework and programming model used for processing huge amounts of data. MapReduce program work in two phases, namely, Map and Reduce. Map tasks deal with splitting and mapping of data while Reduce tasks shuffle and reduce the data.
<img src="/assets/images/20211028_AwesomeDE/pic21.png" class="largepic"/>

Input Splits:

An input to a MapReduce in Big Data job is divided into fixed-size pieces called input splits Input split is a chunk of the input that is consumed by a single map
**Mapping**
This is the very first phase in the execution of map-reduce program. In this phase data in each split is passed to a mapping function to produce output values. In our example, a job of mapping phase is to count a number of occurrences of each word from input splits (more details about input-split is given below) and prepare a list in the form of <word, frequency>
**Shuffling**
This phase consumes the output of Mapping phase. Its task is to consolidate the relevant records from Mapping phase output. In our example, the same words are clubed together along with their respective frequency.
**Reducing**
In this phase, output values from the Shuffling phase are aggregated. This phase combines values from Shuffling phase and returns a single output value. In short, this phase summarizes the complete dataset.
In our example, this phase aggregates the values from Shuffling phase i.e., calculates total occurrences of each word.

# 8.1. What is MapReduce? How it Works - Hadoop MapReduce Tutorial - Guru99


