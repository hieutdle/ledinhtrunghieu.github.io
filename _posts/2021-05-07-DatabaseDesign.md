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
Purpose       support daily transactions                     
Design          application-oriented                            
Data            up-to-date, operational                        
Size            snapshot, gigabytes                             
Queries         simple transactions & frequent updates        
Users           thousands                                       
**OLAP**
Purpose     report and analyze data
Design     subject-oriented (certain subject that's under analysis)
Data        consolidated, historical (over a large period of time, consolidated for long-term analysis.)
Size         archive, terabytes (large amount)
Queries    complex, aggregate queries & limited updates
Users           hundreds

**Working togerther**


<img src="/assets/images/20210507_DatabaseDesign/pic1.png" class="largepic"/>

* OLAP and OLTP systems work together; in fact, they need each other. OLTP data is usually stored in an operational database that is pulled and cleaned to create an OLAP data warehouse
* Without transactional data, no analyses can be done in the first place. Analyses from OLAP systems are used to inform business practices and day-to-day activity, thereby influencing the OLTP databases.


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
* Run b**ig data analytics** using services such as **Apache Spark** and **Hadoop**
    * Useful for deep learning and data discovery because activities require so much data

<img src="/assets/images/20210507_DatabaseDesign/pic5.png" class="largepic"/>

**ETL**

<img src="/assets/images/20210507_DatabaseDesign/pic6.png" class="largepic"/>

**ELT**

<img src="/assets/images/20210507_DatabaseDesign/pic7.png" class="largepic"/>

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

# 2. Database Schemas and Normalization

Learn to implement star and snowflake schemas, recognize the importance of normalization and see how to normalize databases to different extents.

# 2.1. Star and snowflake schema

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
