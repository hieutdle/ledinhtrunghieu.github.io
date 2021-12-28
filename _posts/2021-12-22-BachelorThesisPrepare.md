---
layout: post
author: ledinhtrunghieu
title: Preparation for Bachelor Thesis
---

# 1. OLAP vs OLTP

## 1.1 OLTP

* Online Transaction Processing
* The most traditional processing system. 
* Able to manage transaction-oriented applications and can be characterized by a large number of short, atomic database operations, such as inserts, updates, and deletes, 
* Common in your day-to-day application. 

Examples: 
1. Online banking and e-commerce applications.
2. Find the price of a book
3. Update latest customer transaction
4. Keep track of employee hours

Purpose: support daily transactions
Design: application-oriented
Data: up-to-date, operational
Size: snapshot, gigabytes
Queries: simple transactions & frequent updates
Users: thousands


## 1.2 OLAP

Online Analytical Processing, manages historical or archival data. It is characterized by a relatively low volume of transactions. OLAP systems are typically used for analytical purposes — to extract insights and knowledge from bulk data, merged from multiple sources. Unlike OLTP, the goal for OLAP systems is to have a limited amount of transactions, each consisting of mostly bulk reads and writes. Data warehouses are the typical infrastructure to maintain these systems.

Example: 
1. Calculate books with best profit margin
2. Find most loyal customers
3. Decide employee of the month

Purpose: report and analyze data 
Design: Object-oriented (certain subject that’s under analysis) 
Data: consolidated, historical (over a large period of time, consolidated for long-term analysis.) 
Size: archive, terabytes (large amount) 
Queries: complex, aggregate queries & limited updates Users: hundreds

<img src="/assets/images/20212212_BachelorThesis/pic1.png" class="largepic"/>

## 1.3 Row vs Columnar 

Row-oriented databases are commonly used in OLTP systems, whereas columnar ones are more suitable for OLAP.

Row-oriented databases store the whole row in the same block, if possible. Columnar databases store columns in subsequent blocks.

<img src="/assets/images/20212212_BachelorThesis/pic2.png" class="largepic"/>

<img src="/assets/images/20212212_BachelorThesis/pic2.png" class="largepic"/>

**Perfomance**

For the analytical query:

What is the average age of males?

We would have these possibilities:

Row wise DB -> Has to read all the data in the database — 100GB to read.
Columnar DB -> Has to read only the columns age and gender — 2GB to read.

But Row database performs fast multiple read, write, update, delete operations	

https://dzone.com/articles/the-data-processing-holy-grail-row-vs-columnar-dat

# 2. Star vs Snowflake schema

## 2.1. Star Schema
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


## 2.2. Snowflake schema 

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

## 2.3. Which schema is better for performance?
The Star schema is in a more de-normalized form and hence tends to be better for performance. Along the same lines the Star schema uses less foreign keys so the query execution time is limited. In almost all cases the data retrieval speed of a Star schema has the Snowflake beat.

## 2.4. Which schema is better for readability?
The Star schema is easier for readability because its query structure is not as complex, on the other hand the Snowflake has a complex query structure and is tougher for readability and implementing changes. The changes to be implemented can be tougher to put into a Snowflake schema because of the tendency to have a lot of joins in the query structure. The Star schema on the other hand uses less joins and tends to have more data redundancy. So for readability the schema to go with would be the star schema.

## 2.5. Which schema is better for maintainability?
Maintainability for a data warehouse is heavily dependent on the amount of redundant data. The more redundancies the more places the maintenance needs to take place. Out of the two schemas the Snowflake has the least data redundancies so is hence the more maintainable choice.

## 2.6. Snowflake vs Star Schema
Now comes a major question that a developer has to face before starting to design a data warehouse. Snowflake or Star schema? We’ve gone over the difference and the choice needs to be made on a case by case basis, but two important factors outside of the above, which are more personal choices, are the number of dimensions in your data and the size of the data.

If the data is relatively small and the end result is more of a DataMart than a data warehouse, then the choice tends to lean towards Star schema. Along the same line, if the relationships inside the data are simple and don’t have many to many relationships then the choice tends to lean towards Star Schema. On the other hand, if you are building a bigger solution with many to many relationships then going with the Snowflake is your best bet.

Another thing that needs to be considered is the number of dimensions in your dimension table. If a single dimension requires more than one table, it’s better to use the Snowflake schema. For example, a star schema would use one date dimension but a Snowflake, can have Dimension date tables that extends out to dimension day of the week, quarter, month…etc. If these branches or snowflakes are needed than the Star schema isn’t the way to go.

# 3. Cloud

Companies can process data in their own data center, often on **premises**. 

Server on **premises**:
* Bought (racks of servers)
* Need space (a room to store)
* Inconvenient (if we move offices, we have to transport servers without losing service)
* Electrical and maintenance cost

Processing tasks can be more or less intense, and don't happen continuously. 
* Companies would need to provide enough processing power for peak moments
* Processing power unused at quieter times (at quieter times, much of the processing power would remain unused)

It's avoiding this waste of resources that makes **cloud** computing so appealing.

Server on **cloud**:
* Rented server (the rent is cheap)
* Don't need space (don't need a room to store)
* Use the resources we need, at the time we need them
* The closer the server is to the user, the less latency they will experience when using our application

**Cloud computing for data storage**:
* Database reliability: Running a data-critical company, we have to prepare for the worst. A fire can break out in an on-premises data center. To be safe, we need to replicate our data at a different geographical location. 
* Risk with sensitive data: If your company manipulates sensitive or confidential data, there is a risk associated with someone else hosting it, and government surveillance. 

With the advent of big data, companies specializing in these kinds of issues, called cloud providers, were born.
The three big players, in decreasing order of market share, are Amazon Web Services, Microsoft Azure, and Google Cloud.

Storage services allow you to upload files of all types to the cloud
Computation services allow you to perform computations on the cloud.
Database services is a database that typically runs on the cloud.

<img src="/assets/images/20210422_DEForEveryone/pic10.png" class="largepic"/>

# 4. Amazon Web Services

# 5. Data processing - Batch, MapReduce, Streaming

# 6. Pipeline / Workflow Management


