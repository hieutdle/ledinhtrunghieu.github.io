---
layout: post
author: ledinhtrunghieu
title: Lesson 16 - Database Design
---

# 1. Processing, Storing, and Organizing Data

Learn about the two approaches to data processing, OLTP and OLAP. Get familiar with the different forms data can be stored in and learn the basics of data modeling.

## 1.1. OLTP and OLAP

**How should we organize and manage data?**
* **Schemas**: How should my data be logically organized?
* **Normalization**: Should my data have minimal dependency and redundancy?
* **Views**: What joins will be done most often?
* **Access control**: Should all users of the data have the same level of access
* **DBMS**: How do I pick between all the SQL and noSQL options?

**OLTP**
* Online Transaction Processing
* Find the price of a book
* Update latest customer transaction 
* Keep track of employee hours

**OLAP**
* Online Analytical Processing
* Calculate books with best profit margin 
* Find most loyal customers
* Decide employee of the month

**OLTP vs OLAP**

**OLTP** 
* Purpose: support daily transactions                     
* Design: application-oriented                            
* Data: up-to-date, operational                        
* Size: snapshot, gigabytes                             
* Queries: simple transactions & frequent updates        
* Users: thousands

**OLAP**
Purpose: report and analyze data
Design: ubject-oriented (certain subject that's under analysis)
Data: consolidated, historical (over a large period of time, consolidated for long-term analysis.)
Size: archive, terabytes (large amount)
Queries: complex, aggregate queries & limited updates
Users: hundreds

**Working togerther**


<img src="/assets/images/20210507_DatabaseDesign/pic1.png" class="largepic"/>

* OLAP and OLTP systems work together; in fact, they need each other. OLTP data is usually stored in an operational database that is pulled and cleaned to create an OLAP data warehouse
* Without transactional data, no analyses can be done in the first place. Analyses from OLAP systems are used to inform business practices and day-to-day activity, thereby influencing the OLTP databases.

<img src="/assets/images/20210507_DatabaseDesign/pic69.png" class="largepic"/>

## 1.2. Storing data

1. **Structured data**
    * Follows a schema
    * Defined data types & relationships
    * _e.g., SQL, tables in a relational database _

2. **Unstructured data**
    * Schemaless
    * Makes up most of data in the world
    * e.g., photos, chat logs, MP3

3. **Semi-structured data**
    * Does not follow larger schema 
    * Self-describing structure
    * e.g., NoSQL, XML, JSON

<img src="/assets/images/20210507_DatabaseDesign/pic70.png" class="largepic"/>

<img src="/assets/images/20210507_DatabaseDesign/pic1.png" class="largepic"/>

Because its clean and organized, structured data is easier to analyze. However, it's not as flexible because it needs to follow a schema, which makes it less scalable.

**Storing data beyond traditional databases**
* **Traditional databases**
    * For storing real-time relational structured data. **OLTP**
* **Data warehouses**
    * For analyzing archived structured data. **OLAP**
* **Data lakes**
    * For storing data of all structures = exibility and scalability 
    * For analyzing **big data**

**Data warehouses**
* Optimized for analytics - OLAP
    * Organized for reading/aggregating data 
    * Usually read-only
* Contains data from multiple sources 
* Massively Parallel Processing (MPP) for faster query
* Typically uses a denormalized schema and dimensional modeling

<img src="/assets/images/20210507_DatabaseDesign/pic3.png" class="largepic"/>

**Data mart**
* Subset of data warehouses 
* Dedicated to a specific topic
* Data marts allow departments to have easier access to the data that matters to them.

<img src="/assets/images/20210507_DatabaseDesign/pic4.png" class="largepic"/>


**Data lake**
* Store **all types** of data at **a lower cost**:
    * It uses object storage as opposed to the traditional block or file storage. This allows massive amounts of data to be stored effectively of all types, from streaming data to operational database
    * e.g., raw, operational databases, IoT device logs, real-time, relational and non-relational 
* Retains all data and can take up petabytes
* Schema-on-read as opposed to schema-on-write
* Need to catalog data otherwise becomes a **data swamp**
* Run **big data analytics** using services such as **Apache Spark** and **Hadoop**
    * Useful for deep learning and data discovery because activities require so much data


Technically, traditional databases and warehouses can store unstructured data, but not cost-effectively. 
Data Lake storage is cheaper because it uses object storage as opposed to the traditional block or file storage. This allows massive amounts of data to be stored effectively of all types, from streaming data to operational databases. 
Lakes are massive because they store all the data that might be used. Data lakes are often petabytes in size - that's 1,000 terabytes! 
Unstructured data is the most scalable, which permits this size. Lakes are schema-on-read, meaning the schema is created as data is read. Warehouses and traditional databases are classified as schema-on-write because the schema is predefined.

**Data lakes aren't only limited to storage. It's becoming popular to run analytics on data lakes. This is especially true for tasks like deep learning and data discovery, which needs a lot of data that doesn't need to be that "clean."**
<img src="/assets/images/20210507_DatabaseDesign/pic5.png" class="largepic"/>

**Extract, Transform, Load or Extract, Load, Transform**

ETL is the more traditional approach for warehousing and smaller-scale analytics. But, ELT has become common with big data projects. In ETL, data is transformed before loading into storage - usually to follow the storage's schema, as is the case with warehouses. 
In ELT, the data is stored in its native form in a storage solution like a data lake.**Portions of data are transformed for different purposes**, from building a data warehouse to doing deep learning.

**ETL**

<img src="/assets/images/20210507_DatabaseDesign/pic6.png" class="largepic"/>

**ELT**

<img src="/assets/images/20210507_DatabaseDesign/pic7.png" class="largepic"/>

<img src="/assets/images/20210507_DatabaseDesign/pic71.png" class="largepic"/>

**Ordering ETL Tasks**

<img src="/assets/images/20210507_DatabaseDesign/pic72.png" class="largepic"/>

**When should you choose a data warehouse over a data lake?**
To create accessible and isolated data repositories for other analysts. Analysts will appreciate working in a data warehouse more because of its organization of structured data that make analysis easier.

## 1.3. Database design
* Determines how data is logically stored
    * How is data going to be queried, read and updated?
* Uses **database models**: high-level specifications for database structure 
    * Most popular: relational model: It defines rows as records and columns as attributes. It calls for rules such as each row having unique keys.
    * Some other options: NoSQL models, object-oriented model, network model
* Uses **schemas**: blueprint of the database
    * Defines tables, fields, relationships, indexes, and views
    * When inserting data in relational databases, schemas must be respected

**Data modeling**
Process of creating a data model for the data to be stored
1. **Conceptual data model**: describes entities, relationships, and attributes
    * Tools: data structure diagrams, e.g., entity-relational diagrams and UML diagrams
2. **Logical data model**: defines tables, columns, relationships
    * Tools: database models and schemas, e.g., relational model and star schema
3. **Physical data model**: describes physical storage
    * Tools: partitions, CPUs, indexes, backup systems and tablespaces

**Conceptual - ER diagram**
<img src="/assets/images/20210507_DatabaseDesign/pic8.png" class="largepic"/>
Entities, relationships, and attributes

**Logical - schema**
<img src="/assets/images/20210507_DatabaseDesign/pic9.png" class="largepic"/>
Fastest conversion: entities become the tables

**Dimensional modeling**
Adaptation of the relational model for data warehouse design
* Optimized for OLAP queries: aggregate data, not updating (OLTP) 
* Built using the star schema
* Easy to interpret and extend schema

**Elements of dimensional modeling**
<img src="/assets/images/20210507_DatabaseDesign/pic10.png" class="largepic"/>

* The turquoise table is a fact table called songs. It contains foreign keys to purple dimension tables. These dimension tables expand on the attributes of a fact table, such as the album it is in and the artist who made it. 
* The records in fact tables often change as new songs get inserted. 
* Albums, labels, artists, and genres will be shared by more than one song - hence records in dimension tables won't change as much

**Fact tables**
* Decided by business use-case 
* Holds records of a metric 
* Changes regularly
* Connects to dimensions via foreign keys

**Dimension tables**
* Holds descriptions of attributes 
* Does not change as often

<img src="/assets/images/20210507_DatabaseDesign/pic73.png" class="largepic"/>

```sql
SELECT 
	-- Get the total duration of all runs
	SUM(duration_mins)
FROM 
	runs_fact
-- Get all the week_id's that are from July, 2019
INNER JOIN week_dim ON week_dim.week_id = runs_fact.week_id
WHERE month = 'July' and year = '2019';
```
# 2. Database Schemas and Normalization

Learn to implement star and snowflake schemas, recognize the importance of normalization and see how to normalize databases to different extents.

## 2.1. Star and snowflake schema

**Dimesion modeling: Star schema**

**Fact tables**
* Decided by business use-case 
* Holds records of a metric 
* Changes regularly
* Connects to dimensions via foreign keys

**Dimension tables**
* Holds descriptions of attributes 
* Does not change as often

**Example**
* Supply books to stores in USA and Canada
* Keep track of book sales

**Star schema example**

<img src="/assets/images/20210507_DatabaseDesign/pic11.png" class="largepic"/>

**Snowflake schema (an extension)**

<img src="/assets/images/20210507_DatabaseDesign/pic12.png" class="largepic"/>

Snow ake schemas: more than one dimension. Because dimension tables are **normalized**

**What is normalization?**
* Database design technique
* Divides tables into smaller tables and connects them via relationships 
* **Goal**: reduce redundancy and increase data integrity
**Identify repeating groups of data and create new tables for them**

**Book dimension of the star schema**
Most likely to have repeating values:
* Author 
* Publisher
* Genre

<img src="/assets/images/20210507_DatabaseDesign/pic13.png" class="largepic"/>

**Book dimension of the snowflake schema**

<img src="/assets/images/20210507_DatabaseDesign/pic14.png" class="largepic"/>

**Store dimension of the star schema**

<img src="/assets/images/20210507_DatabaseDesign/pic15.png" class="largepic"/>

* City
* State 
* Country

**Store dimension of the snowflake schema**

<img src="/assets/images/20210507_DatabaseDesign/pic16.png" class="largepic"/>

<img src="/assets/images/20210507_DatabaseDesign/pic17.png" class="largepic"/>

**Adding foreign keys**

<img src="/assets/images/20210507_DatabaseDesign/pic18.png" class="largepic"/>

```sql
-- Add the book_id foreign key
ALTER TABLE fact_booksales ADD CONSTRAINT sales_book
    FOREIGN KEY (book_id) REFERENCES dim_book_star (book_id);
    
-- Add the time_id foreign key
ALTER TABLE fact_booksales ADD CONSTRAINT sales_time
   FOREIGN KEY (time_id) REFERENCES dim_time_star (time_id);
    
-- Add the store_id foreign key
ALTER TABLE fact_booksales ADD CONSTRAINT sales_store
   FOREIGN KEY (store_id) REFERENCES dim_store_star (store_id);
```

**Extending the book dimension**

<img src="/assets/images/20210507_DatabaseDesign/pic19.png" class="largepic"/>

```sql
-- Create a new table for dim_author with an author column
CREATE TABLE dim_author (
    author varchar(256)  NOT NULL
);

-- Insert authors 
INSERT INTO dim_author
SELECT DISTINCT author FROM dim_book_star;

-- Add a primary key 
ALTER TABLE dim_author ADD COLUMN author_id SERIAL PRIMARY KEY;

-- Output the new table
SELECT * FROM dim_author;
```

## 2.2. Normalized and denormalized databases

**Denormalized: star schema**
**Goals: get quantity of all Octavia E. Butler books sold in Vancouver in Q4 of 2018**
```sql
SELECT SUM(quantity) FROM fact_booksales
    -- Join to get city	
    INNER JOIN dim_store_star on fact_booksales.store_id = dim_store_star.store_id
    -- Join to get author	
    INNER JOIN dim_book_star on fact_booksales.book_id = dim_book_star.book_id
    -- Join to get year and quarter	
    INNER JOIN dim_time_star on fact_booksales.time_id = dim_time_star.time_id
WHERE
    dim_store_star.city = 'Vancouver' 
    AND dim_book_star.author = 'Octavia E. Butler' 
    AND dim_time_star.year = 2018 
    AND dim_time_star.quarter = 4;
```

**Normalized: snowflake schema**
```sql
SELECT
    SUM(fact_booksales.quantity) 
FROM
    fact_booksales
    -- Join to get city
    INNER JOIN dim_store_sf ON fact_booksales.store_id = dim_store_sf.store_id 
    INNER JOIN dim_city ON dim_store_sf.city_id = dim_city_sf.city_id
    -- Join to get author
    INNER JOIN dim_book_sf ON fact_booksales.book_id = dim_book_sf.book_id 
    INNER JOIN dim_author_sf ON	dim_book_sf.author_id = dim_author_sf.author_id
    -- Join to get year and quarter
    INNER JOIN dim_time_sf ON fact_booksales.time_id = dim_time_sf.time_id 
    INNER JOIN dim_month_sf ON dim_time_sf.month_id = dim_month_sf.month_id
    INNER JOIN dim_quarter_sf ON dim_month_sf.quarter_id = dim_quarter_sf.quarter_id 
    INNER JOIN dim_year_sf ON dim_quarter_sf.year_id = dim_year_sf.year_id
WHERE
    dim_city_sf.city = `Vancouver` AND
    dim_author_sf.author = `Octavia E. Butler` AND
    dim_year_sf.year = 2018 AND dim_quarter_sf.quarter = 4;
```

3 joins vs 8 joins
So, why would we want to normalize a databases?

**Normalization saves space**

<img src="/assets/images/20210507_DatabaseDesign/pic20.png" class="largepic"/>

Denormalized databases enables data redundancy

**Normalization saves space**

<img src="/assets/images/20210507_DatabaseDesign/pic21.png" class="largepic"/>

Normalization eliminates data redundancy

**Normalization ensures better data integrity**
1. Enforces data consistency
    Must respect naming conventions because of referential integrity, e.g., 'California', not 'CA' or 'california'
2. Safer updating, removing, and inserting
    Less data redundancy = less records to alter
3. Easier to redesign by extending
    Smaller tables are easier to extend than larger tables

**Database normalization**

**Advantages**
* Normalization eliminates data redundancy: save on storage 
* Better data integrity: accurate and consistent data

**Disadvantages**
* Complex queries require more CPU

**Normalization on OLTP and OLAP**

**OLTP**
* e.g., Operational databases 
* Typically highly normalized
* Write-intensive
* Prioritize quicker and safer insertion of data

**OLAP**
* e.g., Data warehouses 
* Typically less normalized
* Read-intensive
* Prioritize quicker queries for analytics

**Querying the star schema**

<img src="/assets/images/20210507_DatabaseDesign/pic22.png" class="largepic"/>

```sql
-- Output each state and their total sales_amount
SELECT dim_store_star.state, sum(sales_amount)
FROM fact_booksales
	-- Join to get book information
    JOIN dim_book_star on fact_booksales.book_id = dim_book_star.book_id
	-- Join to get store information
    JOIN dim_store_star on fact_booksales.store_id = dim_store_star.store_id
-- Get all books with in the novel genre
WHERE  
    dim_book_star.genre = 'novel'
-- Group results by state
GROUP BY
    dim_store_star.state;
```

**Querying the snowflake schema**

<img src="/assets/images/20210507_DatabaseDesign/pic23.png" class="largepic"/>

```sql
-- Output each state and their total sales_amount
SELECT dim_state_sf.state, sum(sales_amount)
FROM fact_booksales
    -- Joins for the genre
    JOIN dim_book_sf on fact_booksales.book_id = dim_book_sf.book_id
    JOIN dim_genre_sf on dim_book_sf.genre_id = dim_genre_sf.genre_id
    -- Joins for the state 
    JOIN dim_store_sf on fact_booksales.store_id = dim_store_sf.store_id 
    JOIN dim_city_sf on dim_store_sf.city_id = dim_city_sf.city_id
	JOIN dim_state_sf on  dim_city_sf.state_id = dim_state_sf.state_id
-- Get all books with in the novel genre and group the results by state
WHERE  
    dim_genre_sf.genre = 'novel'
GROUP BY
   dim_state_sf.state;
```
**Updating**
```sql
-- Output records that need to be updated in the star schema
SELECT * FROM dim_store_star
WHERE country != 'USA' AND country !='CA';

--Extending the snowflake schema

-- Add a continent_id column with default value of 1
ALTER TABLE dim_country_sf
ADD continent_id int NOT NULL DEFAULT(1);

-- Add the foreign key constraint
ALTER TABLE dim_country_sf ADD CONSTRAINT country_continent
   FOREIGN KEY (continent_id) REFERENCES dim_continent_sf(continent_id);
   
-- Output updated table
SELECT * FROM dim_country_sf;

```

## 2.3. Normal forms

**Normalization**
Identify repeating groups of data and create new tables for them 
A more formal definition:
    * The goals of normalization are to:
        * Be able to characterize the level of redundancy in a relational schema 
        * Provide mechanisms for transforming schemas in order to remove redundancy

**Normal forms (NF)**
Ordered from least to most normalized:
* First normal form (1NF) 
* Second normal form (2NF) 
* Third normal form (3NF)
* Elementary key normal form (EKNF) 
* Boyce-Codd normal form (BCNF)
* Fourth normal form (4NF) 
* Essential tuple normal form (ETNF)
* Fifth normal form (5NF)
* Domain-key Normal Form (DKNF) 
* Sixth normal form (6NF)

**1NF rules**
* Each record must be unique - no duplicate rows 
* Each cell must hold one value

**Initial data**
<img src="/assets/images/20210507_DatabaseDesign/pic24.png" class="largepic"/>

All these rows are unique, but the courses_completed column has more than one course in two records. To rectify this, we can split the original table as such. Now, all the records are unique and each column has one value.

<img src="/assets/images/20210507_DatabaseDesign/pic25.png" class="largepic"/>

**2NF**
* Must satisfy 1NF AND
    * If primary key is one column
        * then automatically satisfies 2NF
    * If there is a composite primary key
        * then each non-key column must be dependent on all the keys 

<img src="/assets/images/20210507_DatabaseDesign/pic26.png" class="largepic"/>

First is the instructor, which isn't dependent on the student_id - only the course_id. Meaning an instructor solely depends on the course, not the students who take the course. The same goes for the instructor_id column. However, the percent completed is dependent on both the student and the course id.

<img src="/assets/images/20210507_DatabaseDesign/pic27.png" class="largepic"/>

**3NF**
* Satisfies 2NF
* No transitive dependencies: non-key columns can't depend on other non-key columns 

<img src="/assets/images/20210507_DatabaseDesign/pic28.png" class="largepic"/>

Instructor_id and Instructor definitely depend on each other. 

<img src="/assets/images/20210507_DatabaseDesign/pic29.png" class="largepic"/>

**Data anomalies**

What is risked if we don't normalize enough? Why we would want to put effort into normalizing a database even more? Why isn't 1NF enough? A database that isn't normalized enough is prone to three types of anomaly errors:
1. **Update anomaly**
2. **Insertion anomaly**
3. **Deletion anomaly**

**Update anomaly**
**Data inconsistency caused by data redundancy when updating**
<img src="/assets/images/20210507_DatabaseDesign/pic30.png" class="largepic"/>
To update student `520`'s email:
* Need to update more than one record, otherwise, there will be inconsistency 
* User updating needs to know about redundancy

This is a simple example - as we scale, it's harder to keep track of these redundancies.

**Insertion anomaly**
**Unable to add a record due to missing attributes**
<img src="/assets/images/20210507_DatabaseDesign/pic31.png" class="largepic"/>

An insertion anomaly is when you are unable to add a new record due to missing attributes. For example, if a student signs up for DataCamp, but doesn't start any courses, they cannot be put into this database

**Deletion anomaly**
**Deletion of record(s) causes unintentional loss of data**
<img src="/assets/images/20210507_DatabaseDesign/pic32.png" class="largepic"/>

If you were to delete any of these students, you would lose the course information provided in the columns enrolled_in and taught_by. This could be resolved if we put that information in another table.


Does the customers table meet 1NF criteria?

No, because there are multiple values in cars_rented and invoice_id
<img src="/assets/images/20210507_DatabaseDesign/pic74.png" class="largepic"/>

```sql

--1NF

-- Create a new table to hold the cars rented by customers
CREATE TABLE cust_rentals (
  customer_id INT NOT NULL,
  car_id VARCHAR(128) NULL,
  invoice_id VARCHAR(128) NULL
);

-- Drop a column from customers table to satisfy 1NF
ALTER TABLE customers
DROP COLUMN cars_rented,
DROP COLUMN invoice_id;

-- Create a new table to satisfy 2NF
CREATE TABLE cars (
  car_id VARCHAR(256) NULL,
  model VARCHAR(128),
  manufacturer VARCHAR(128),
  type_car VARCHAR(128),
  condition VARCHAR(128),
  color VARCHAR(128)
);
```

Why doesn't customer_rentals meet 2NF criteria?
Because there are non-key attributes describing the car that only depend on one primary key, car_id.
<img src="/assets/images/20210507_DatabaseDesign/pic75.png" class="largepic"/>

```sql
-- 2NF
-- Drop columns in customer_rentals to satisfy 2NF
ALTER TABLE customer_rentals
DROP COLUMN model,
DROP COLUMN manufacturer, 
DROP COLUMN type_car,
DROP COLUMN condition,
DROP COLUMN color;
```

<img src="/assets/images/20210507_DatabaseDesign/pic76.png" class="largepic"/>

Why doesn't rental_cars meet 3NF criteria?


Because there are two columns that depend on the non-key column, model.

```sql
--3NF

-- Create a new table to satisfy 3NF
CREATE TABLE car_model(
  model VARCHAR(128),
  manufacturer VARCHAR(128),
  type_car VARCHAR(128)
);

-- Drop columns in rental_cars to satisfy 3NF
ALTER TABLE rental_cars
DROP COLUMN manufacturer, 
DROP COLUMN type_car;


```

# 3. Database Views

Create and query views. Identifythe difference between materialized and non-materialized views.

## 3.1. Database Views

In a database, a **view** is the result set of a stored query on the data, which the database users can query just as they would in a persistent database collection object

**Views** are **virtual table** that is not part of the physical schema
* Query, not data, is stored in memory
* Data is aggregated from data in the same tables
* When the view is created, can be queried like a regular database table
* No need to retype common queries, add virtual table without altering the database's schemas

**Create a view (syntax)**
```sql
CREATE VIEW view_name AS
SELECT col1, col2 
FROM table_name 
WHERE condition;
```
<img src="/assets/images/20210507_DatabaseDesign/pic33.png" class="largepic"/>

Goal: Return titles and authors of the `science fiction` genre

```sql
CREATE VIEW scifi_books AS
SELECT title, author, genre
FROM dim_book_sf
JOIN dim_genre_sf ON dim_genre_sf.genre_id = dim_book_sf.genre_id 
JOIN dim_author_sf ON dim_author_sf.author_id = dim_book_sf.author_id 
WHERE dim_genre_sf.genre = 'science fiction';
```
**Querying a view (example)**

```sql
SELECT * FROM scifi_books
```
<img src="/assets/images/20210507_DatabaseDesign/pic34.png" class="largepic"/>

```sql
SELECT * FROM scifi_books
```
**=**
```sql
CREATE VIEW scifi_books AS
SELECT title, author, genre
FROM dim_book_sf
JOIN dim_genre_sf ON dim_genre_sf.genre_id = dim_book_sf.genre_id 
JOIN dim_author_sf ON dim_author_sf.author_id = dim_book_sf.author_id 
WHERE dim_genre_sf.genre = 'science fiction';
```

**Viewing views (in PostgreSQL)**

Includes system views
```sql
SELECT * FROM INFORMATION_SCHEMA.views;
```

Excludes system views
```sql
SELECT * FROM information_schema.views
WHERE table_schema NOT IN ('pg_catalog', 'information_schema');
```
**Benefits of views**
* Doesn't take up storage
* A form of **access control**
    * Hide sensitive columns and restrict what user can see
* Masks complexity of queries
    * Useful for highly normalized schemas

<img src="/assets/images/20210507_DatabaseDesign/pic35.png" class="largepic"/>

<img src="/assets/images/20210507_DatabaseDesign/pic36.png" class="largepic"/>

**Practice**
```sql
--Viewing views
-- Get all non-systems views
SELECT * FROM information_schema.views
WHERE table_schema NOT IN ('pg_catalog', 'information_schema');

-- Creating and querying a view

-- Create a view for reviews with a score above 9
CREATE VIEW high_scores AS
SELECT * FROM REVIEWS
WHERE score > 9;

-- Count the number of self-released works in high_scores
SELECT COUNT(*) FROM high_scores
INNER JOIN labels ON high_scores.reviewid = labels.reviewid
WHERE label = 'self-released';
```
## 3.2. Managing views

**Creating more complex views**
Aggregation: `SUM()`,`AVG()`,`COUNT()`,`MIN()`,`MAX`,`GROUP BY`, etc	
Joins: `INNER JOIN`,`LEFT JOIN`,`RIGHT JOIN`,`FULL JOIN`
Conditional: `WHERE`,`HAVING`,`UNIQUE`,`NOT NULL`,`AND`,`OR`,`>`,`<`,etc

**Granting and revoking access to a view**
`GRANT privilege(s)` or `REVOKE privilege(s)`
`ON object`
`TO role` or `FROM role`
* **Privileges**: `SELECT`,`INSERT`,`UPDATE`,`DELETE`, etc
* **Objects**: table, view, schema, etc
* **Roles**: a database user or a group of database users

**Example:**

```sql
GRANT UPDATE ON ratings TO PUBLIC;
```
The update privilege on an object called ratings is being granted to public. PUBLIC is a SQL term that encompasses all users. All users can now use the UPDATE command on the ratings object.
```sql
REVOKE INSERT ON films FROM db_user;
```
the user db_user will no longer be able to INSERT on the object films

**Updating a view**
```sql
UPDATE films SET kind = 'Dramatic' WHERE kind = 'Drama';
```
If you remember correctly, a view isn't a physical table. Therefore, when you run an update, you are updating the tables behind the view.
Not all views are updatable. There are criteria for a view to be considered updatable. The criteria depend on the type of SQL being used.
* View is made up of one table
* Doesn't use a window or aggregate function

**Inserting into a view**
```sql
INSERT INTO films (code, title, did, date_prod, kind)  
    VALUES ('T_601', 'Yojimbo', 106, '1961-06-16', 'Drama');
```
When you run an insert command into a view, you're again really inserting into the table behind it. The criteria for inserting is usually very similar to updatable views.
Not all views are insertable
Avoid modifying data through views

**Dropping a view**
```sql
DROP VIEW view_name [CASADE|RESTRICT]
```
* `RESTRICT`: (default): returns an error if there are objects that depend on the view
* `CASCADE`: drops view and any object that depends on that view

**Redefining a view**
```sql
CREATE OR REPLACE VIEW view_name AS new_query
```
* If a view with `view_name` exists, it is replaced
* `new_query` must generate the same column names, order, and data types as the old query
* The column output may be different
* New columns may be added at the end
If these criteria can't be met, drop the existing view and create a new one

**Altering a view**

```sql
ALTER VIEW [ IF EXISTS ] name ALTER [ COLUMN ] column_name SET DEFAULT expression
ALTER VIEW [ IF	EXISTS ] name ALTER [ COLUMN ] column_name DROP DEFAULT
ALTER VIEW [ IF	EXISTS ] name OWNER TO new_owner
ALTER VIEW [ IF	EXISTS ] name RENAME TO new_name
ALTER VIEW [ IF	EXISTS ] name SET SCHEMA new_schema
ALTER VIEW [ IF	EXISTS ] name SET ( view_option_name [= view_option_value] [, ...
ALTER VIEW [ IF	EXISTS ] name RESET ( view_option_name [, ... ] )
```

**Creating a view from other views**
```sql
-- Create a view with the top artists in 2017
CREATE VIEW top_artists_2017 AS
-- with only one column holding the artist field
SELECT artist_title.artist FROM artist_title
INNER JOIN top_15_2017
ON artist_title.reviewid = top_15_2017.reviewid;

-- Output the new view
SELECT * FROM top_artists_2017;

artist

massive attack
krallice
uranium club
liliput
kleenex
taso
various artists
little simz
yotam avni
brian eno
harry bertoia
run the jewels
steven warwick
pete rock
smoke dza
various artists
senyawa
```

**Granting and revoking access**
```sql
-- Revoke everyone's update and insert privileges
REVOKE UPDATE, INSERT ON long_reviews FROM PUBLIC; 

-- Grant editor update and insert privileges 
GRANT UPDATE, INSERT ON long_reviews TO editor; 
```
**Redefining a view**
```sql
-- Redefine the artist_title view to have a label column
CREATE OR REPLACE VIEW artist_title AS
SELECT reviews.reviewid, reviews.title, artists.artist, labels.label
FROM reviews
INNER JOIN artists
ON artists.reviewid = reviews.reviewid
INNER JOIN labels
ON labels.reviewid = reviews.reviewid;

SELECT * FROM artist_title;
```

## 3.3. Materialized views

**Two types of views**
**Views**
* Also known as **non-materialized views **
* How we've defined views so far

**Materialized views**
* Physically materialized
* Stores the **query results**, not the **query**
* Querying a materialized view means accessing the stored query results 
    * Not running the query like a non-materialized view
* Refreshed or rematerialized when prompted or scheduled: query is run and the stored query results are updated. This can be scheduled depending on how often you expect the underlying query results are changing.

**When to use materialized views**
* Long running queries (complex long execution time)
* Materialized views allow data scientists and analysts to run long queries and get results very quickly. 
* Underlying query results don't change often
* Data warehouses because OLAP is not write-intensive 
    * Save on computational cost of frequent queries

**Implementing materialized views (in PostgreSQL)**
```sql
CREATE MATERIALIZED VIEW my_mv AS SELECT * FROM existing_table;

REFRESH MATERIALIZED VIEW my_mv;
```

**Managing dependencies**
* Materialized views often depend on other materialized views
* Creates a **dependency chain** when refreshing views
* Not the most effcient to refresh all views at the same time

Unlike non-materialized views, you need to manage when you refresh materialized views when you have dependencies. For example, let's say you have two materialized views: X and Y. Y uses X in its query; meaning Y depends on X. X doesn't depend on Y as it doesn't use Y in its query. Let' s say X has a more time-consuming query. If Y is refreshed before X's refresh is completed, then Y now has out-of-date data. This creates a dependency chain when refreshing views. 

**Dependency example**

<img src="/assets/images/20210507_DatabaseDesign/pic37.png" class="largepic"/>

**Tools for managing dependencies**
* Use Directed Acyclic Graphs (DAGs) to keep track of views
* Pipeline scheduler tools

<img src="/assets/images/20210507_DatabaseDesign/pic38.png" class="largepic"/>

<img src="/assets/images/20210507_DatabaseDesign/pic39.png" class="largepic"/>

<img src="/assets/images/20210507_DatabaseDesign/pic40.png" class="largepic"/>

**Creating and refreshing a materialized view**

```sql
-- Create a materialized view called genre_count 
CREATE MATERIALIZED VIEW genre_count AS
SELECT genre, COUNT(*) 
FROM genres
GROUP BY genre;

INSERT INTO genres
VALUES (50000, 'classical');

-- Refresh genre_count
REFRESH MATERIALIZED VIEW genre_count;

SELECT * FROM genre_count;

genre	count
global	217
experimental	1815
metal	860
null	2367
classical	1
electronic	3874
folk/country	685
pop/r&b	1432
jazz	435
rap	1559
rock	9436
```

**Why do companies use pipeline schedulers, such as Airflow and Luigi, to manage materialized views?**
To refresh materialized views with consideration to dependences between views. These pipeline schedulers help visualize dependencies and create a logical order for refreshing views.

# 4. Database Management

## 4.1. Database roles and access control

* Manage database access permissions
* A database role is an entity that contains information that: 
    * Define the role's privileges
        * Can you login?
        * Can you create databases? 
        * Can you write to tables?
    * Interact with the client authentication system 
        * Password
* Roles can be assigned to one or more users
* Roles are global across a database cluster installation

**Create a role**
* Empty role
```sql
CREATE ROLE data_analyst;
```
* Roles with some attributes set
```sql
CREATE ROLE intern WITH PASSWORD 'PasswordForIntern' VALID UNTIL '2020-01-01';
CREATE ROLE admin CREATEDB;
ALTER ROLE admin CREATEROLE;
```

**GRANT and REVOKE privileges from roles**
```sql
GRANT UPDATE ON ratings TO data_analyst;
REVOKE UPDATE ON ratings FROM data_analyst;
```
The available privileges in PostgreSQL are:
* `SELECT`,	`INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, `REFERENCES`, `TRIGGER`, `CREATE` , `CONNECT`, `TEMPORARY`, `EXECUTE`, and `USAGE`

**Users and groups (are both roles)**
* A role is an entity that can function as a user and/or a group 
    * User roles
    * Group roles

<img src="/assets/images/20210507_DatabaseDesign/pic41.png" class="largepic"/>

Group role

```sql
CREATE ROLE data_analyst;
```

User role

```sql
CREATE ROLE intern WITH PASSWORD 'PasswordForIntern' VALID UNTIL '2020-01-01';
```

```sql
GRANT data_analyst TO alex;

REVOKE data_analyst FROM alex;
```

**Common PostgreSQL roles**

<img src="/assets/images/20210507_DatabaseDesign/pic42.png" class="largepic"/>

**Benefits and pitfalls of roles**
**Benefits**
* Roles live on after users are deleted
* Roles can be created before user accounts 
* Save DBAs time

**Pitfalls**
* Sometimes a role gives a specific user too much access 
    * You need to pay a ention


**Practice**
```sql
-- Create a data scientist role
CREATE ROLE data_scientist;

-- Create a role for Marta
CREATE ROLE marta LOGIN;

-- Create an admin role
CREATE ROLE admin WITH CREATEDB CREATEROLE;

-- Grant data_scientist update and insert privileges
GRANT UPDATE, INSERT ON long_reviews TO data_scientist;

-- Give Marta's role a password
ALTER ROLE marta WITH PASSWORD 's3cur3p@ssw0rd';

-- Add Marta to the data scientist group
GRANT data_scientist TO Marta;

-- Celebrate! You hired data scientists.

-- Remove Marta from the data scientist group
REVOKE data_scientist FROM Marta;
```

## 4.2. Table partitioning

**Why partition?**
*Tables grow (100s Gb / Tb)*

**Problem:** queries/updates become slower 

**Because:** e.g., indices don't fit memory

**Solution:** split table into smaller parts (= partitioning)

**Data modeling refresher**
1. Conceptual data model
2. Logical data model: For partitioning, logical data model is the same
3. Physical data model: Partitioning is part of physical data model

**Vertical partitioning**
<img src="/assets/images/20210507_DatabaseDesign/pic43.png" class="largepic"/>
<img src="/assets/images/20210507_DatabaseDesign/pic44.png" class="largepic"/>

E.g., store	`long_description` on slower medium

**Horizontal partitioning**
<img src="/assets/images/20210507_DatabaseDesign/pic45.png" class="largepic"/>

<img src="/assets/images/20210507_DatabaseDesign/pic46.png" class="largepic"/>

```sql
CREATE TABLE sales (
    ...
    timestamp DATE NOT NULL
)
PARTITION BY RANGE (timestamp);

CREATE TABLE sales_2019_q1 PARTITION OF sales
    FOR VALUES FROM ('2019-01-01') TO ('2019-03-31');

...
CREATE TABLE sales_2019_q4 PARTITION OF sales
    FOR VALUES FROM ('2019-09-01') TO ('2019-12-31');
CREATE INDEX ON sales ('timestamp');
```
**Pros/cons of horizontal partitioning**
**Pros**
* Increasing the chance **heavily-used parts** of the index fit in memory.
* Move rarely accessed partitions to a slower medium
* Used for both OLAP as OLTP
**Cons**
* Partitioning an existing table can be a hassle: you have to create a new table and copy over the data.
* We can not always set the same type of constraints on a partitioned table, for example, the PRIMARY KEY constraint.

**Relation to sharding**
<img src="/assets/images/20210507_DatabaseDesign/pic47.png" class="largepic"/>

We can take partitioning one step further and distribute the partitions over several machines. When horizontal partitioning is applied to spread a table over several machines, it's called sharding. You can see how this relates to massively parallel processing databases, where each node, or machine, can do calculations on specific shards.

**Creating vertical partitions**
```sql
-- Create a new table called film_descriptions
CREATE TABLE film_descriptions (
    film_id INT,
    long_description TEXT
);

-- Copy the descriptions from the film table
INSERT INTO film_descriptions
SELECT film_id, long_description FROM film;
    
-- Drop the column in the original table
ALTER TABLE film DROP COLUMN long_description;

-- Join to create the original table
SELECT * FROM film 
JOIN film_descriptions USING(film_id);
```

**Creating horizontal partitions**
```sql
-- Create a new table called film_partitioned
CREATE TABLE film_partitioned (
  film_id INT,
  title TEXT NOT NULL,
  release_year TEXT
)
PARTITION BY LIST (release_year);

-- Create the partitions for 2019, 2018, and 2017
CREATE TABLE film_2019
	PARTITION OF film_partitioned FOR VALUES IN ('2019');

CREATE TABLE film_2018
	PARTITION OF film_partitioned FOR VALUES IN ('2018');

CREATE TABLE film_2017
	PARTITION OF film_partitioned FOR VALUES IN ('2017');

-- Insert the data into film_partitioned
INSERT INTO film_partitioned
SELECT film_id, title, release_year FROM film;

-- View film_partitioned
SELECT * FROM film_partitioned;
```

<img src="/assets/images/20210507_DatabaseDesign/pic77.png" class="largepic"/>


## 4.3. Data integration

**What is data integration**

**Data Integration** combines data from different sources, formats, technologies to provide users with a translated and unified view of that data.

**Business case examples**
* 360-degree customer view : To see all information departments have about a customer in a unified place
* Acquisition: One company acquiring another, and needs to combine their respective databases.
* Legacy systems: An insurance company with claims in old and new systems, would need to integrate data to query all claims at once.

**Unified data model format**

<img src="/assets/images/20210507_DatabaseDesign/pic48.png" class="largepic"/>

**Update cadence -sales**
<img src="/assets/images/20210507_DatabaseDesign/pic49.png" class="largepic"/>

**Update cadence -air traffic**
<img src="/assets/images/20210507_DatabaseDesign/pic50.png" class="largepic"/>

**Different update cadences**

<img src="/assets/images/20210507_DatabaseDesign/pic51.png" class="largepic"/>

Your sources are in different formats, you need to make sure they can be assembled together.

**Transformations**

<img src="/assets/images/20210507_DatabaseDesign/pic52.png" class="largepic"/>

**Transformation - tools**

<img src="/assets/images/20210507_DatabaseDesign/pic53.png" class="largepic"/>

**Choosing a data integration tool**
* Flexible
* Reliable
* Scalable

**Automated testing and proactive alerts**
<img src="/assets/images/20210507_DatabaseDesign/pic55.png" class="largepic"/>

You should have automated testing and proactive alerts. If any data gets corrupted on its way to the unified data model, the system lets you know. For example, you could aggregate sales data after each transformation and ensure that the total amount remains the same.


**Security**
<img src="/assets/images/20210507_DatabaseDesign/pic56.png" class="largepic"/>

Security is also a concern: if data access was originally restricted, it should remain restricted in the unified data model.Business analysts using the unified data model should not have access to the credit card numbers. You should anonymize the data during ETL so that analysts can only access the first four numbers, to identify the type of card being used.

**Data governance - lineage**
<img src="/assets/images/20210507_DatabaseDesign/pic57.png" class="largepic"/>

For data governance purposes, you need to consider lineage: for effective auditing, you should know where the data originated and where it is used at all times.

<img src="/assets/images/20210507_DatabaseDesign/pic78.png" class="largepic"/>

## 4.4. Picking a Database Management System (DBMS)

**DBMS**
* DataBase Management System 
* Create and maintain databases
    * Data
    * Database schema 
    * Database engine
* Interface between database and end users

<img src="/assets/images/20210507_DatabaseDesign/pic54.png" class="largepic"/>

**DBMS types**
* Choice of DBMS depends on database type
* Two types: 
    * SQL DBMS
    * NoSQL DBMS

**SQL DBMS**
* Relational DataBase Management System (RDBMS)
* Based on the relational model of data 
* Query language: SQL
* Best option when:
    * Data is structured and unchanging 
    * Data must be consistent

<img src="/assets/images/20210507_DatabaseDesign/pic58.png" class="largepic"/>

**NoSQL DBMS**
* Less structured
* Document-centered rather than table-centered
* Data doesn’t have to fit into well-defined rows and columns 
* Best option when:
    * Rapid growth
    * No clear schema definitions 
    * Large quantities of data
* Types: key-value store, document store, columnar database, graph database

**NoSQL DBMS - key-value store**
<img src="/assets/images/20210507_DatabaseDesign/pic59.png" class="largepic"/>

* Combinations of keys and values 
    * Key: unique identifier
    * Value: anything
Use case: managing the shopping cart for an on-line buyer


Example:

<img src="/assets/images/20210507_DatabaseDesign/pic60.png" class="largepic"/>

**NoSQL DBMS - document store**
<img src="/assets/images/20210507_DatabaseDesign/pic61.png" class="largepic"/>

* Similar to key-value
* Values (= documents) are structured 
* Use case: content management 

Example:
<img src="/assets/images/20210507_DatabaseDesign/pic62.png" class="largepic"/>

**NoSQL DBMS - columnar database**
<img src="/assets/images/20210507_DatabaseDesign/pic63.png" class="largepic"/>

* Store data in columns
* Scalable
* Use case: big data analytics where speed is important

Example:
<img src="/assets/images/20210507_DatabaseDesign/pic64.png" class="largepic"/>

**NoSQL DBMS - graph database**

<img src="/assets/images/20210507_DatabaseDesign/pic65.png" class="largepic"/>

* Data is interconnected and best represented as a graph
* Use case: social media data, recommendations

Example:

<img src="/assets/images/20210507_DatabaseDesign/pic66.png" class="largepic"/>

**Choosing a DBMS**

<img src="/assets/images/20210507_DatabaseDesign/pic67.png" class="largepic"/>

If your application has a fixed structure and doesn’t need frequent modifications, a SQL DBMS is preferable. Conversely, if you have applications where data is changing frequently and growing rapidly, like in big data analytics, NoSQL is the best option for you.

<img src="/assets/images/20210507_DatabaseDesign/pic68.png" class="largepic"/>

# 5. Reference

1. [Database Design- DataCamp](https://learn.datacamp.com/courses/database-design)