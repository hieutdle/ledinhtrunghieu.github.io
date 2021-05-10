---
layout: post
author: ledinhtrunghieu
title: Lesson 2 - Introduction to Data Engineering
---

# 1. Introduction to Data Engineering


In this first chapter, you will be exposed to the world of data engineering! Explore the differences between a data engineer and a data scientist, get an overview of the various tools data engineers use and expand your understanding of how cloud technology plays a role in data engineering.

## 1.1. Tasks of Data Engineering 

In comes the data engineer:
* Data is scattered around many databases.
* The data resides in tables that are optimized for applications to run, not for analyses
* Legacy code has caused a lot of the data to be corrupt

Data engineer to the rescue!

<img src="/assets/images/20210501_IntroductiontoDE/pic1.png" class="largepic"/>

**Data Engineer**:
* Extracts data from different sources
* Loads it into one single database ready to use
* Optimized the database scheme so it becomes faster to query. 
* Removed corrupt data

Data Engineer make your life as a data scientist easier.

**Data engineer's Definition**: An engineer that develops, constructs, tests, and maintains architectures such as databases and large-scale processing systems.

**The data engineer nowadays** is focused on processing and handling massive amounts of data, and setting up clusters of machines to do the computing.

**Different between Data Engineer and Data Analyst**

Data Engineer: 
* Develop scalable data architecture
* Streamline data acquisition
* Set up processes to bring together data	
* Clean corrupt data
* Well versed in cloud technology

Data Scientist:
* Mining for patterns in data
* Applying statistical models on large datasets
* Building predictive models using machine learning
* Developing tools to monitor essential business processes
* Cleaning data by removing statistical outliers

## 1.2. Tools of the data engineer

**Databases**
* Computer system that holds large amounts of data (SQL or NoSQL databases)
* Support applications: Applications rely on databases to provide certain functionality. For example, in an online store, a database holds product data like prices or amount in stock. On the other hand, other databases hold data specifically for analyses

Two examples of databases are MySQL or PostgreSQL

<img src="/assets/images/20210501_IntroductiontoDE/pic4.png" class="largepic"/>


**Processing**
* Clean
* Aggregate data
* Join it together from different sources

<img src="/assets/images/20210501_IntroductiontoDE/pic2.png" class="largepic"/>

Typically, huge amounts of data have to be processed. That is where **parallel processing** comes into play. Instead of processing the data on one computer, data engineers use **clusters of machines** to process the data. Often, these tools make an abstraction of the underlying architecture and have a simple API.

Example: 

```
df = spark.read.parquet("user.parquet")

outlier = df.filter([df["age] > 100)

print(outliers.count())
```
It looks a lot like simple pandas filter or count operations. However, behind the curtains, a cluster of computers could be performing these operations using the **PySpark** framework

An example processing tool is Spark or Hive

<img src="/assets/images/20210501_IntroductiontoDE/pic5.png" class="largepic"/>

**Scheduling**:

Scheduling tools help to make sure **data moves** from one place to another **at the correct time**, with a **specific interval**. Data engineers make sure these jobs run in a timely fashion and that they run in the right order. Sometimes processing jobs need to run in a particular order to function correctly. For example, tables from two databases might need to be joined together after they are both cleaned. In the following diagram, the JoinProductOrder job needs to run after CleanProduct and CleanOrder ran.

<img src="/assets/images/20210501_IntroductiontoDE/pic3.png" class="largepic"/>

For scheduling, we can use Apache Airflow, Oozie, or we can use the simple bash tool: cron.

<img src="/assets/images/20210501_IntroductiontoDE/pic6.png" class="largepic"/>

**A Data Pipeline** 

You can think of the data engineering **pipeline** through this diagram. It **extracts** all data through connections with several databases, **transforms** it using a cluster computing framework like **Spark**, and **loads** it into an analytical database. Also, everything is **scheduled** to run in a specific order through a scheduling framework like **Airflow**. A small side note here is that the sources can be external APIs or other file formats too

<img src="/assets/images/20210501_IntroductiontoDE/pic7.png" class="largepic"/>

**Cloud Computing**

You can see my last post about cloud computing [here](https://ledinhtrunghieu.github.io/2021/04/22/DEForEveryone.html#35-cloud-computing) 

# 2. Data Engineering toolbox

Now that you know the primary differences between a data engineer and a data scientist, get ready to explore the data engineer's toolbox! Learn in detail about different types of databases data engineers use, how parallel computing is a cornerstone of the data engineer's toolkit, and how to schedule data processing jobs using scheduling frameworks.


# 2.1. Databases

**Database**
* Store information
* Holds data
* Organizes data
* Retrieve/Search data through DBMS
* A usually large collection of data organized especially for rapid search and retrieval

**Database vs Storage file system**: The main difference between databases and simple storage systems like file systems is the **level of organization** and the fact that the database management systems abstract away a lot of **complicated data operations** like search, replication and much more. File systems host less such functionality.

**Structured, semi-structured and unstructured data**

Structured:
* Structured data is coherent to a well-defined structure
* Database schemas usually define structure
* Example of structured data: tabular data in a relational database

Unstructured:
* Schemaless
* Looks like files (photographs or videos)

Semi-structured:
* Between Structured and Unstructured
* Example: JSON File

**SQL and NoSQL**

**SQL**
* Tables form the data
* Database schema defines relations and properties between these tables
* Relational databases
* MySQL and PostgreSQL

**NoSQL**
* Non-relational databases
* Structured or unstructured
* Key-value stores (e.g. caching or distributed configuration)
* Document DB (e.g. JSON objects: structured or unstructured object)
* Redis (Key-value) and MongoDB (Document)

**The Database schema**

<img src="/assets/images/20210501_IntroductiontoDE/pic8.png" class="largepic"/>

**Star schema**

<img src="/assets/images/20210501_IntroductiontoDE/pic9.png" class="largepic"/>

* In **data warehousing**, a schema you'll see often is the **star schema**. A lot of analytical databases like **Redshift** have optimizations for these kinds of schemas
* The star schema consists of one or more fact **tables** referencing any number of **dimension tables**
* **Fact tables** contain records that represent things that happened in the world, like orders. 
* **Dimension tables** hold information on the world itself, like customer names or product prices.

Example:

The database schema

<img src="/assets/images/20210501_IntroductiontoDE/pic10.png" class="largepic"/>

```
data = pd.read_sql("""
SELECT first_name, last_name FROM "Customer"
ORDER BY last_name, first_name
""", db_engine)

# Show the first 3 rows of the DataFrame
print(data.head(3))

# Show the info of the DataFrame
print(data.info())
```

Joining on relation

```
data = pd.read_sql("""
SELECT * FROM "Customer"
INNER JOIN "Order"
ON "Order"."customer_id"="Customer"."id"
""", db_engine)

# Show the id column of data
print(data["id"])
```
# 2.2. Parallel computing 

Introduction to parallel computing [here](https://ledinhtrunghieu.github.io/2021/04/22/DEForEveryone.html#34-parallel-computing) 

Example:

<img src="/assets/images/20210501_IntroductiontoDE/pic10.png" class="largepic"/>

Let's look into a more practical example. We're starting with a dataset of all Olympic events from 1896 until 2016. From this dataset, you want to get an average age of participants for each year. For this example, let's say you have four processing units at your disposal. You decide to distribute the load over all of your processing units. To do so, you need to split the task into smaller subtasks. In this example, the average age calculation for each group of years is as a subtask. You can achieve that through 'groupby.' Then, you distribute all of these subtasks over the four processing units. This example illustrates roughly how the first distributed algorithms like Hadoop MapReduce work, the difference being the processing units are distributed over several machines.


**multiprocessing.Pool**

```
from multiprocessing import Pool

def take_mean_age(year_and_group): 
    year, group = year_and_group
    return pd.DataFrame({"Age": group["Age"].mean()}, index=[year])

with Pool(4) as	p:
    results = p.map(take_mean_age, athlete_events.groupby("Year"))

result_df = pd.concat(results)
```
We could use the 'multiprocessing.Pool API' to distribute work over several cores on the same machine. We have a function 'take_mean_age', which accepts a tuple: the year of the group and the group itself as a DataFrame. 'take_mean_age' returns a DataFrame with one observation and one column: the mean age of the group. 
The resulting DataFrame is indexed by year. We can then take this function, and map it over the groups generated by '.groupby()', using the '.map()' method of 'Pool'. By defining '4' as an argument to 'Pool', the mapping runs in 4 separate processes, and thus uses 4 cores. Finally, we can concatenate the results to form the resulting DataFrame.

**Dask**

Several packages offer a layer of abstraction to avoid having to write such low-level code. For example, the 'dask' framework offers a DataFrame object, which performs a groupby and apply using multiprocessing out of the box. You need to define the number of partitions, for example, '4'. 'dask' divides the DataFrame into 4 parts, and performs '.mean()' within each part separately. Because dask' uses lazy evaluation, you need to add '.compute()' to the end of the chain.

```
import dask.dataframe as dd

# Partition dataframe into 4
athlete_events_dask = dd.from_pandas(athlete_events, npartitions = 4)

# Run parallel computations on each partition
result_df = athlete_events_dask.groupby('Year').Age.mean().compute()
```

**From task to subtasks**

