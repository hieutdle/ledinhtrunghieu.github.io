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

                **OLTP**                                        **OLAP**
Purpose         support daily transactions                      report and analyze data
Design          application-oriented                            subject-oriented (certain subject that's under analysis)
Data            up-to-date, operational                         consolidated, historical (over a large period of time, consolidated for long-term analysis.)
Size            snapshot, gigabytes                             archive, terabytes (large amount)
Queries         simple transactions & frequent updates          complex, aggregate queries & limited updates
Users           thousands                                       hundreds

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

