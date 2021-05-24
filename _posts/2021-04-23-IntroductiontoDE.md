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

<img src="/assets/images/20210423_IntroductiontoDE/pic1.png" class="largepic"/>

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

<img src="/assets/images/20210423_IntroductiontoDE/pic4.png" class="largepic"/>


**Processing**
* Clean
* Aggregate data
* Join it together from different sources

<img src="/assets/images/20210423_IntroductiontoDE/pic2.png" class="largepic"/>

Typically, huge amounts of data have to be processed. That is where **parallel processing** comes into play. Instead of processing the data on one computer, data engineers use **clusters of machines** to process the data. Often, these tools make an abstraction of the underlying architecture and have a simple API.

Example: 

```python
df = spark.read.parquet("user.parquet")

outlier = df.filter(df["age"] > 100)

print(outliers.count())
```
It looks a lot like simple pandas filter or count operations. However, behind the curtains, a cluster of computers could be performing these operations using the **PySpark** framework

An example processing tool is Spark or Hive

<img src="/assets/images/20210423_IntroductiontoDE/pic5.png" class="largepic"/>

**Scheduling**:

Scheduling tools help to make sure **data moves** from one place to another **at the correct time**, with a **specific interval**. Data engineers make sure these jobs run in a timely fashion and that they run in the right order. Sometimes processing jobs need to run in a particular order to function correctly. For example, tables from two databases might need to be joined together after they are both cleaned. In the following diagram, the JoinProductOrder job needs to run after CleanProduct and CleanOrder ran.

<img src="/assets/images/20210423_IntroductiontoDE/pic3.png" class="largepic"/>

For scheduling, we can use Apache Airflow, Oozie, or we can use the simple bash tool: cron.

<img src="/assets/images/20210423_IntroductiontoDE/pic6.png" class="largepic"/>

**A Data Pipeline** 

You can think of the data engineering **pipeline** through this diagram. It **extracts** all data through connections with several databases, **transforms** it using a cluster computing framework like **Spark**, and **loads** it into an analytical database. Also, everything is **scheduled** to run in a specific order through a scheduling framework like **Airflow**. A small side note here is that the sources can be external APIs or other file formats too

<img src="/assets/images/20210423_IntroductiontoDE/pic7.png" class="largepic"/>

**Cloud Computing**

You can see my last post about cloud computing [here](https://ledinhtrunghieu.github.io/2021/04/22/DEForEveryone.html#35-cloud-computing) 

# 2. Data Engineering toolbox

Now that you know the primary differences between a data engineer and a data scientist, get ready to explore the data engineer's toolbox! Learn in detail about different types of databases data engineers use, how parallel computing is a cornerstone of the data engineer's toolkit, and how to schedule data processing jobs using scheduling frameworks.


## 2.1. Databases

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

<img src="/assets/images/20210423_IntroductiontoDE/pic8.png" class="largepic"/>

**Star schema**

<img src="/assets/images/20210423_IntroductiontoDE/pic9.png" class="largepic"/>

* In **data warehousing**, a schema you'll see often is the **star schema**. A lot of analytical databases like **Redshift** have optimizations for these kinds of schemas
* The star schema consists of one or more fact **tables** referencing any number of **dimension tables**
* **Fact tables** contain records that represent things that happened in the world, like orders. 
* **Dimension tables** hold information on the world itself, like customer names or product prices.

Example:

The database schema

<img src="/assets/images/20210423_IntroductiontoDE/pic10.png" class="largepic"/>

```python
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

```python
data = pd.read_sql("""
SELECT * FROM "Customer"
INNER JOIN "Order"
ON "Order"."customer_id"="Customer"."id"
""", db_engine)

# Show the id column of data
print(data["id"])
```
## 2.2. Parallel computing 

Introduction to parallel computing [here](https://ledinhtrunghieu.github.io/2021/04/22/DEForEveryone.html#34-parallel-computing) 

Example:

<img src="/assets/images/20210423_IntroductiontoDE/pic10.png" class="largepic"/>

Let's look into a more practical example. We're starting with a dataset of all Olympic events from 1896 until 2016. From this dataset, you want to get an average age of participants for each year. For this example, let's say you have four processing units at your disposal. You decide to distribute the load over all of your processing units. To do so, you need to split the task into smaller subtasks. In this example, the average age calculation for each group of years is as a subtask. You can achieve that through `groupby.` Then, you distribute all of these subtasks over the four processing units. This example illustrates roughly how the first distributed algorithms like Hadoop MapReduce work, the difference being the processing units are distributed over several machines.


**multiprocessing.Pool**

```python
from multiprocessing import Pool

def take_mean_age(year_and_group): 
    year, group = year_and_group
    return pd.DataFrame({"Age": group["Age"].mean()}, index=[year])

with Pool(4) as	p:
    results = p.map(take_mean_age, athlete_events.groupby("Year"))

result_df = pd.concat(results)
```
We could use the `multiprocessing.Pool API` to distribute work over several cores on the same machine. We have a function `take_mean_age`, which accepts a tuple: the year of the group and the group itself as a DataFrame. `take_mean_age` returns a DataFrame with one observation and one column: the mean age of the group. 
The resulting DataFrame is indexed by year. We can then take this function, and map it over the groups generated by `.groupby()`, using the `.map()` method of `Pool`. By defining `4` as an argument to `Pool`, the mapping runs in 4 separate processes, and thus uses 4 cores. Finally, we can concatenate the results to form the resulting DataFrame.

**Dask**

Several packages offer a layer of abstraction to avoid having to write such low-level code. For example, the `dask` framework offers a DataFrame object, which performs a groupby and apply using multiprocessing out of the box. You need to define the number of partitions, for example, `4`. `dask` divides the DataFrame into 4 parts, and performs `.mean()` within each part separately. Because `dask` uses lazy evaluation, you need to add `.compute()` to the end of the chain.

```python
import dask.dataframe as dd

# Partition dataframe into 4
athlete_events_dask = dd.from_pandas(athlete_events, npartitions = 4)

# Run parallel computations on each partition
result_df = athlete_events_dask.groupby('Year').Age.mean().compute()
```

**From task to subtasks**

For this example, you will be using parallel computing to apply the function `take_mean_age()` that calculates the average athlete's age in a given year in the Olympics events dataset. The DataFrame athlete_events has been loaded for you and contains amongst others, two columns:
* Year: the year the Olympic event took place
* Age: the age of the Olympian

You will be using the multiprocessor.Pool API which allows you to distribute your workload over several processes. The function `parallel_apply()` is defined in the sample code. It takes in as input the function being applied, the grouping used, and the number of cores needed for the analysis. Note that the @print_timing decorator is used to time each operation.

```python
# Function to apply a function over multiple cores
@print_timing
def parallel_apply(apply_func, groups, nb_cores):
    with Pool(nb_cores) as p:
        results = p.map(apply_func, groups)
    return pd.concat(results)

# Parallel apply using 1 core
parallel_apply(take_mean_age, athlete_events.groupby('Year'),1)

# Parallel apply using 2 cores
parallel_apply(take_mean_age, athlete_events.groupby('Year'), 2)

# Parallel apply using 4 cores
parallel_apply(take_mean_age, athlete_events.groupby('Year'), 4)
```

**Using a DataFrame**

In the previous example, you saw how to split up a task and use the low-level python multiprocessing.Pool API to do calculations on several processing units.

It's essential to understand this on a lower level, but in reality, you'll never use this kind of APIs. A more convenient way to parallelize an apply over several groups is using the dask framework and its abstraction of the pandas DataFrame, for example.

```python
import dask.dataframe as dd

# Set the number of partitions
athlete_events_dask = dd.from_pandas(athlete_events, npartitions = 4)

# Calculate the mean Age per Year
print(athlete_events_dask.groupby('Year').Age.mean().compute())
```

## 2.3 Parallel computing framework

**Hadoop**

<img src="/assets/images/20210423_IntroductiontoDE/pic12.png" class="largepic"/>

**Hadoop** is a collection of open source projects, maintained by the Apache Software Foundation. Some of them are a bit outdated, but it's still relevant to talk about them. There are two Hadoop projects we want to focus on: **MapReduce** and **HDFS**.

**HDFS**

<img src="/assets/images/20210423_IntroductiontoDE/pic13.png" class="largepic"/>

**HDFS** is a **Hadoop Distributed File System**. It's similar to the **file system** you have on your computer, the only difference being **the files reside on multiple different computers**. HDFS has been essential in the big data world, and for parallel computing by extension. Nowadays, cloud-managed storage systems like Amazon S3 often replace HDFS.

**MapReduce**

<img src="/assets/images/20210423_IntroductiontoDE/pic14.png" class="largepic"/>

**MapReduce** was one of the first popularized  **big-data processing paradigms**. It works similar to the previous example, where the program splits tasks into subtasks, distributing the workload and data between several processing units. For MapReduce, these processing units are several computers in the cluster. MapReduce had its flaws; one of it was that it was hard to write these MapReduce jobs. Many software programs popped up to address this problem, and one of them was Hive.

**Hive**

<img src="/assets/images/20210423_IntroductiontoDE/pic15.png" class="largepic"/>

**Hive** is a layer on top of the Hadoop ecosystem that makes data from several sources queryable in a structured way using Hive's SQL variant: Hive SQL. Facebook initially developed Hive, but the Apache Software Foundation now maintains the project. Although MapReduce was initially responsible for running the Hive jobs, it now integrates with several other data processing tools.

Hive Exmaple: 

```sql
SELECT year, AVG(age) 
FROM views.athlete_events 
GROUP BY year
```

<img src="/assets/images/20210423_IntroductiontoDE/pic16.png" class="largepic"/>

This Hive query selects the average age of the Olympians per Year they participated. As you'd expect, this query looks indistinguishable from a regular SQL query. However, behind the curtains, this query is transformed into a job that can operate on a cluster of computers.

**Spark**

**Spark** **distributes** data processing tasks between clusters of computers. While MapReduce-based systems tend to need expensive disk writes between jobs, Spark tries to **keep as much processing as possible in memory**. In that sense, Spark was also an answer to the limitations of MapReduce. The disk writes of MapReduce were especially limiting in interactive exploratory data analysis, where each step builds on top of a previous step. Spark originates from the University of California, where it was developed at the Berkeley's AMPLab. Currently, the Apache Software Foundation maintains the project.

**Resilient distributed datasets (RDD)**

Spark's architecture relies on something called **resilient distributed datasets**, or **RDDs**. Without diving into technicalities, this is a data structure that maintains data which is distributed between multiple nodes. Unlike DataFrames, RDDs don't have named columns. From a conceptual perspective, you can think of RDDs as lists of tuples. We can do two types of operations on these data structures: transformations, like map or filter, and actions, like count or first. Transformations result in transformed RDDs, while actions result in a single result.

**PySpark**

When working with Spark, people typically use a programming language interface like PySpark. PySpark is the Python interface to spark. There are interfaces to Spark in other languages, like R or Scala, as well. PySpark hosts a DataFrame abstraction, which means that you can do operations very similar to pandas DataFrames. PySpark and Spark take care of all the complex parallel computing operations.

Have a look at the following PySpark example. Similar to the Hive Query you saw before, it calculates the mean age of the Olympians, per Year of the Olympic event. Instead of using the SQL abstraction, like in the Hive Example, it uses the DataFrame abstraction.

```python
# Load the dataset into athlete_events_spark first
(athlete_events_spark
.groupBy('Year')
.mean('Age')
.show())
```

simillar to

```sql
SELECT year, AVG(age) 
FROM views.athlete_events 
GROUP BY year
```

Compare Hadoop vs PySpark vs Hive

<img src="/assets/images/20210423_IntroductiontoDE/pic18.png" class="largepic"/>

**PySpark Example**

In this example, You'll use the PySpark package to handle a Spark DataFrame. The data is the same as in previous exercises: participants of Olympic events between 1896 and 2016.
The Spark Dataframe, athlete_events_spark is available in your workspace.

```python
# Print the type of athlete_events_spark
print(type(athlete_events_spark))

# Print the schema of athlete_events_spark
print(athlete_events_spark.printSchema())

# Group by the Year, and find the mean Age
print(athlete_events_spark.groupBy('Year').mean('Age'))

# Group by the Year, and find the mean Age
print(athlete_events_spark.groupBy('Year').mean('Age').show())
```

**Running PySpark files**

```
cat /home/repl/spark-script.py
```

Result

```
repl:~$ cat /home/repl/spark-script.py
from pyspark.sql import SparkSession


if __name__ == "__main__":
    spark = SparkSession.builder.getOrCreate()
    athlete_events_spark = (spark
        .read
        .csv("/home/repl/datasets/athlete_events.csv",
             header=True,
             inferSchema=True,
             escape='"'))

    athlete_events_spark = (athlete_events_spark
        .withColumn("Height",
                    athlete_events_spark.Height.cast("integer")))

    print(athlete_events_spark
        .groupBy('Year')
        .mean('Height')
        .orderBy('Year')
        .show())
```

Submit

```
spark-submit \
  --master local[4] \
  /home/repl/spark-script.py
```
Output: A DataFrame with average Olympian heights by year.

## 2.4. Workflow scheduling frameworks

An example of pipeline

<img src="/assets/images/20210423_IntroductiontoDE/pic19.png" class="largepic"/>

You can write a Spark job that pulls data from a CSV file, filters out some corrupt records, and loads the data into a SQL database ready for analysis. However, you need to do this every day as new data is coming in to the CSV file. 

One option is to run the job each day manually: Doesn't scale well: Weekends?

There are simple tools that could solve this problem, like cron, the Linux tool. However, let's say you have one job for the CSV file and another job to pull in and clean the data from an API, and a third job that joins the data from the CSV and the API together. The third job depends on the first two jobs to finish. It quickly becomes apparent that we need a more holistic approach, and a simple tool like cron won't suffice. So, we ended up with dependencies between jobs.

**DAGs**

<img src="/assets/images/20210423_IntroductiontoDE/pic20.png" class="largepic"/>

**A DAG (Directed Acyclic Graphs)** is a set of nodes that are connected by directed edges. There are no cycles in the graph, which means that no path following the directed edges sees a specific node more than once. In the example on the slide, Job A needs to happen first, then Job B, which enables Job C and D and finally Job E. As you can see, it feels natural to represent this kind of workflow in a DAG. The jobs represented by the DAG can then run in a daily schedule, for example.

Tools for the jobs: Luigi, cron, Apache Airflow

**Apache Airlow**

<img src="/assets/images/20210423_IntroductiontoDE/pic21.png" class="largepic"/>

Airbnb created **Airflow** as an internal tool for workflow management. They open-sourced Airflow in 2015, and it later joined the Apache Software Foundation in 2016. They built Airflow around the concept of DAGs. Using Python, developers can create and test these DAGs that build up complex pipelines.

<img src="/assets/images/20210423_IntroductiontoDE/pic22.png" class="largepic"/>

The first job starts a Spark cluster. Once it's started, we can pull in customer and product data by running the ingest_customer_data and ingest_product_data jobs. Finally, we aggregate both tables using the enrich_customer_data job which runs after both ingest_customer_data and ingest_product_data complete.

```python
#Create the DAG object
dag = DAG(dag_id="example_dag", ..., schedule_interval="0 * * * *")

# Define operations
start_cluster = StartClusterOperator(task_id="start_cluster", dag=dag) 
ingest_customer_data = SparkJobOperator(task_id="ingest_customer_data", dag=dag) 
ingest_product_data = SparkJobOperator(task_id="ingest_product_data", dag=dag) 
enrich_customer_data = PythonOperator(task_id="enrich_customer_data", ..., dag = dag)

# Set up dependency flow 
start_cluster.set_downstream(ingest_customer_data) 
ingest_customer_data.set_downstream(enrich_customer_data) 
ingest_product_data.set_downstream(enrich_customer_data)
```

First, we create a DAG using the `DAG` class. Afterward, we use an Operator to define each of the jobs. Several kinds of operators exist in Airflow. There are simple ones like `BashOperator` and `PythonOperator` that execute bash or Python code, respectively. Then there are ways to write your own operator, like the `SparkJobOperator` or `StartClusterOperator` in the example. Finally, we define the connections between these operators using `.set_downstream()`.

**Airflow DAGs**

In Airflow, a pipeline is represented as a Directed Acyclic Graph or DAG. The nodes of the graph represent tasks that are executed. The directed connections between nodes represent dependencies between the tasks.

<img src="/assets/images/20210423_IntroductiontoDE/pic23.png" class="largepic"/>


```python
# Create the DAG object.
# First, the DAG needs to run on every hour at minute 0. Every hour at minute N would be N * * * *. 

dag = DAG(dag_id="car_factory_simulation",
          default_args={"owner": "airflow","start_date": airflow.utils.dates.days_ago(2)},
          schedule_interval="0 * * * *")

# Task definitions
assemble_frame = BashOperator(task_id="assemble_frame", bash_command='echo "Assembling frame"', dag=dag)
place_tires = BashOperator(task_id="place_tires", bash_command='echo "Placing tires"', dag=dag)
assemble_body = BashOperator(task_id="assemble_body", bash_command='echo "Assembling body"', dag=dag)
apply_paint = BashOperator(task_id="apply_paint", bash_command='echo "Applying paint"', dag=dag)

# Complete the downstream flow
assemble_frame.set_downstream(place_tires)
assemble_frame.set_downstream(assemble_body)
assemble_body.set_downstream(apply_paint)
```
# 3. Extract, Transform and Load (ETL)

Having been exposed to the toolbox of data engineers, it's now time to jump into the bread and butter of a data engineer's workflow! With ETL, you will learn how to extract raw data from various sources, transform this raw data into actionable insights, and load it into relevant databases ready for consumption!

## 3.1. Extract

Very roughly, **Extract** means extracting data from persistent storage, which is not suited for data processing, into memory. Persistent storage could be a file on Amazon S3, for example, or a SQL database. It's the necessary stage before we can start transforming the data. The sources to extract from vary.

<img src="/assets/images/20210423_IntroductiontoDE/pic24.png" class="largepic"/>

**Extract from text files**

First of all, we can extract data from plain text files. These are files that are generally readable for people. They can be unstructured, like a chapter from a book like Moby Dick. Alternatively, these can be **flat files**, where each row is a record, and each column is an attribute of the records. In the latter, we represent data in a tabular format. Typical examples of flat files are comma-, or tab-separated files: `.csv` or `.tsv`. They use commas (,) or tabs respectively to separate columns.

Another widespread data format is called **JSON**, or JavaScript Object Notation. JSON files hold information in a semi-structured way. It consists of 4 atomic data types: number, string, boolean and null. There are also 2 composite data types: array and object. You could compare it to a dictionary in Python. JSON objects can be very nested, like in this example. There's a pretty good mapping from JSON objects to dictionaries in Python. There's a package in the standard library called `json`, which helps you parse JSON data. The function `json.loads` helps you with this. The reason JSON got popular is that in recent days, many web services use this data format to communicate data.

```
{ 
    "an_object": {
        "nested": [
            "one",
            "two",
            "three",
            {
            "key": "four"
            }
        ]
    }
}
```

```python
import json

result = json.loads('{  "key_1": "value_1",
                        "key_2":"value_2"}')

print(result["key_1"])
```

**Data on the Web**

<img src="/assets/images/20210423_IntroductiontoDE/pic25.png" class="largepic"/>

On the web, most communication happens to something called `requests.` You can look at a `request` as a 'request for data.' A `request` gets a `response`. For example, if you browse Google in your web browser, your browser requests the content of the Google home page. Google servers respond with the data that makes up the page.

**Data on the Web through APIs**

However, some web servers don't serve web pages that need to be readable by humans. Some serve data in a JSON data format. We call these servers **APIs** or **application programming interfaces**.

The popular social media tool, Twitter, hosts an API that provides us with information on tweets in JSON format. Using the Twitter API does not tell us anything about how Twitter stores its data; it merely provides us with a **structured way of querying their data**. 

Example:
* Twitter API
```
{ "statuses": [{ "created_at": "Mon May 06 20:01:29 +0000 2019", "text": "this is a tweet"}] }
```

Another example request to the Hackernews API and the resulting JSON response

Hackernews API

```python
import requests

response = requests.get("https://hacker-news.firebaseio.com/v0/item/16222426.json")
print(response.json())
```

```
{'by': 'neis', 'descendants': 0, 'id': 16222426, 'score': 17, 'time': 1516800333, 'title':	}
``` 

You can use the Python package, `requests` to request an API. We will use the `.get()` method and pass an URL. The resulting response object has a built-in helper method called `.json()` to parse the incoming JSON and transform it into a Python object.

**Data in databases**
The most common way of data extraction is extraction from existing application databases. Most applications, like web services, need a database to back them up and persist data. At this point, it's essential to make a distinction between **two main database types**.

**Application Databases**: Databases that applications like web services use, are typically optimized for having lots of **transactions**. **A transaction** typically changes or inserts rows, or records, in the database. For example, let's say we have a customer database. Each row, or record, represents data for one specific customer. A transaction could add a customer to the database, or change their address. These kinds of transactional databases are called **OLTP**, or **online transaction processing**. They are typically **row-oriented**, in which the system adds data per rows. 

**Analytical Databases**: In contrast, databases optimized for analysis are called **OLAP**, or **online analytical processing**. They are often **column-oriented**. 

**Extraction from databases**

To extract data from a database in Python, you'll always need a **connection string**. The **connection string** or **connection URI** is a string that holds information on how to connect to a database. It typically contains the database type, for example, PostgreSQL, the username and password, the host and port and the database name. In Python, you'd use this connection URI with a package like `sqlalchemy` to create a database engine. We can pass this engine object to several packages that support it to interact with the database. The example shows the usage with `pandas`.

Connection string/URI
```
postgresql://[user[:password]@][host][:port]
```
Use in Python

```python
import sqlalchemy
connection_uri = "postgresql://repl:password@localhost:5432/pagila" db_engine = sqlalchemy.create_engine(connection_uri)
import pandas as pd
pd.read_sql("SELECT * FROM customer", db_engine)
```

**Fetch from an API**

You've seen that you can extract data from an API by sending a request to the API and parsing the response which was in JSON format. In this example, you'll be doing the same by using the `requests` library to send a request to the Hacker News API.

[Hacker News](https://news.ycombinator.com/) is a social news aggregation website, specifically for articles related to computer science or the tech world in general. Each post on the website has a JSON representation, which you'll see in the response of the request in the exercise.

```python
import requests

# Fetch the Hackernews post
resp = requests.get("https://hacker-news.firebaseio.com/v0/item/16222426.json")

# Print the response parsed as JSON
print(resp.json())

# Assign the score of the test to post_score
post_score = resp.json()["score"]
print(post_score)
```

**Read from a database**

In this example, you're going to extract data that resides inside tables of a local PostgreSQL database. The data you'll be using is the Pagila example database. The database backs a fictional DVD store application, and educational resources often use it as an example database.

You'll be creating and using a function that extracts a database table into a pandas DataFrame object. The tables you'll be extracting are:
* film: the films that are rented out in the DVD store.
* customer: the customers that rented films at the DVD store.
In order to connect to the database, you'll have to use a PostgreSQL connection URI, which looks something like this:
```
postgresql://[user[:password]@][host][:port][/database]
```

```python
# Function to extract table to a pandas DataFrame
def extract_table_to_pandas(tablename, db_engine):
    query = "SELECT * FROM {}".format(tablename)
    return pd.read_sql(query, db_engine)

# Connect to the database using the connection URI
connection_uri = "postgresql://repl:password@localhost:5432/pagila" 
db_engine = sqlalchemy.create_engine(connection_uri)

# Extract the film table into a pandas DataFrame
extract_table_to_pandas("film", db_engine)

# Extract the customer table into a pandas DataFrame
extract_table_to_pandas("customer", db_engine)
```
## 3.2 Transform


**Kind of transformations**

<img src="/assets/images/20210423_IntroductiontoDE/pic26.png" class="largepic"/>

* Selection of specific attribute. For example, we could select the 'email' column only. 
* Translation of code values. For instance, 'New York' could be translated into 'NY'. 
* Data validation. For example, if 'created_at' does not contain a date value, we could drop the record. 
* Splititng columns into multiple columns
* Joining from multiple sources

**An example: split (Pandas)**

<img src="/assets/images/20210423_IntroductiontoDE/pic27.png" class="largepic"/>

```python
customer_df # Pandas DataFrame with customer data
# Split email column into 2 columns on the '@' 
symbol split_email = customer_df.email.str.split("@", expand=True) 
# At this point, split_email will have 2 columns, a first  
# one with everything before @, and a second one with
# everything after @

# Create 2 new columns using the resulting DataFrame. 
customer_df = customer_df.assign(
    username=split_email[0], 
    domain=split_email[1],
)
```

**Transforming in PySpark**

Extract data into PySpark

```python
import pyspark.sql

spark = pyspark.sql.SparkSession.builder.getOrCreate()

spark.read.jdbc("jdbc:postgresql://localhost:5432/pagila", 
                "customer",
                properties={"user":"repl","password":"password"})
```

The last transformation example will be using PySpark. We could just as well have used pandas if the load is small. However, since we used PySpark, the extract phase needs to load the table into Spark. We can do this with `spark.read.jdbc`, where `spark` is a `SparkSession` object. **JDBC** is a piece of software that helps Spark connect to several relational databases. There are some differences between this connection URI and the one you saw in the previous video. First of all, it's prepended by 'jdbc:', to tell Spark to use JDBC. Second, we pass authorization information in the `properties` attribute instead of the URL. Finally, we pass the name of the table as a second argument.

**Join (PySpark)**

<img src="/assets/images/20210423_IntroductiontoDE/pic28.png" class="largepic"/>

```python
customer_df # PySpark DataFrame with customer data
ratings_df # PySpark DataFrame with ratings data

# Groupby ratings
ratings_per_customer = ratings_df.groupBy("customer_id").mean("rating")

# Join on customer ID 
customer_df.join(
    ratings_per_customer, 
    customer_df.customer_id==ratings_per_customer.customer_id
)
```

We have to add the mean rating for each customer as an attribute to the customer table.  First, we aggregate the ratings by grouping by customer ids using the `.groupBy()` method. To get the mean rating per customer, we chain the groupby method with the `.mean()` method. Afterward, we can join the aggregated table with the customer table. That gives us the customer table, extended with the mean rating for each customer. 

Example: Splitting the rental rate

Suppose you would want to have a better understanding of the rates users pay for movies, so you decided to divide the rental_rate column into dollars and cents.
In this example, you will use the same techniques used in the previous example. The film table has been loaded into the pandas DataFrame film_df. 

```python
# Get the rental rate column as a string
rental_rate_str = film_df.rental_rate.astype(str)

# Split up and expand the column
rental_rate_expanded = rental_rate_str.str.split("." ,expand=True)

# Assign the columns to film_df
film_df = film_df.assign(
    rental_rate_dollar=rental_rate_expanded[0],
    rental_rate_cents=rental_rate_expanded[1],
)
```

* Use the `.astype()` method to convert the `rental_rate` column into a column of string objects, and assign the results to `rental_rate_str`.
* Split `rental_rate_str` on '.' and expand the results into columns. Assign the results to `rental_rate_expanded`.
* Assign the newly created columns into `films_df` using the column names `rental_rate_dollar` and `rental_rate_cents` respectively.

Joining with ratings

In this example, you're going to create more synergies between the film and ratings tables.

```python
# Use groupBy and mean to aggregate the column
ratings_per_film_df = rating_df.groupby("film_id").mean("rating")

# Join the tables using the film_id column
film_df_with_ratings = film_df.join(
    ratings_per_film_df,
    film_df.film_id==rating_df.film_id
)

# Show the 5 first results
print(film_df_with_ratings.show(5))
```

* Take the mean rating per film_id, and assign the result to ratings_per_film_df.
* Complete the .join() statement to join on the film_id column.
* Show the first 5 results of the resulting DataFrame.

## 3.3 Loading

As I mentioned before, there are two different types of databases.

**Analytics or Application databases**

**Analytics:**
* Aggregate queries
* **Online analytic processing (OLAP)**
* Column-oriented

**Applications**
* Lots of transactions
* **Online transaction processing (OLTP)**
* Row-oriented

**Column- and row-oriented**

**Column-oriented**:  In a column-oriented database, we store data per column.  There are multiple reasons why this is optimal for analytics. Without getting too technical, you can think of analytical queries to be mostly about a small subset of columns in a table. By storing data per column, it's faster to loop over these specific columns to resolve a query. In a row-oriented system, we would lose time skipping unused columns for each row. Column-oriented databases also lend themselves better to parallelization.
**Row-oriented**: Most application databases are row-oriented. That means we store data per record, which makes it easy to add new rows in small transactions. For example, in a row-oriented database, adding a customer record is easy and fast.

**Massive Parallel Processing Databases (MPP Databases)**

<img src="/assets/images/20210423_IntroductiontoDE/pic29.png" class="largepic"/>

Massively parallel processing databases is often a target at the end of an ETL process. They're column-oriented databases optimized for analytics, that run in a distributed fashion. Specifically, this means that queries are not executed on a single compute node, but rather split into subtasks and distributed among several nodes. 

Famous example:
* Amazon Redshift
* Azure SQL Data Warehouse
* Google BigQuery

**An example: Redshift**

To load data into Amazon Redshift, an excellent way to do this would be to write files to S3, AWS's file storage service, and send a copy query to Redshift. Typically, MPP databases load data best from files that use a columnar storage format. CSV files would not be a good option, for example. We often use a file format called **parquet** for this purpose.
For example, in pandas, you can use the `.to_parquet()` method on a dataframe. In PySpark, you can use `.write.parquet()`. You can then connect to Redshift using a PostgreSQL connection URI and copy the data from S3 into Redshift.

```python
# Pandas .to_parquet() method 
df.to_parquet("./s3://path/to/bucket/customer.parquet") 
# PySpark .write.parquet() method
df.write.parquet("./s3://path/to/bucket/customer.parquet")
```

```
COPY customer
FROM 's3://path/to/bucket/customer.parquet' FORMAT as parquet
...
```

**Load to PostgreSQL**

In other cases, you might want to load the result of the transformation phase into a PostgreSQL database. For example, your data pipeline could extract from a rating table, transform it to find recommendations and load them into a PostgreSQL database, ready to be used by a recommendation service. For this, there are also several helper methods in popular data science packages. For example, you could use `.to_sql()` in Pandas. Often, you can also provide a strategy for when the table already exists. Valid strategies for `.to_sql()` in Pandas are: "fail", "replace" and "append".

pandas.to_sql()
```python
# Transformation on data
recommendations = transform_find_recommendatins(ratings_df)
# Load into PostgreSQL database 
recommendations.to_sql("recommendations",
                        db_engine,
                        schema="store", 
                        if_exists="replace")
```

Example

Writing to a file

Files are often loaded into a MPP database like Redshift in order to make it available for analysis. The typical workflow is to write the data into columnar data files. These data files are then uploaded to a storage system and from there, they can be copied into the data warehouse. In case of Amazon Redshift, the storage system would be S3, for example. The first step is to write a file to the right format. For this example you'll choose the Apache Parquet file format. There's a PySpark DataFrame called film_sdf and a pandas DataFrame called film_pdf in the workspace.

```python
# Write the pandas DataFrame to parquet
film_pdf.to_parquet("films_pdf.parquet")

# Write the PySpark DataFrame to parquet
film_sdf.write.parquet("films_sdf.parquet")
```

Load into Postgres

You'll write out some data to a PostgreSQL data warehouse. That could be useful when you have a result of some transformations, and you want to use it in an application. For example, the result of a transformation could have added a column with film recommendations, and you want to use them in your online store. There's a pandas DataFrame called film_pdf in your workspace.
As a reminder, here's the structure of a connection URI for sqlalchemy: 
```
postgresql://[user[:password]@][host][:port][/database]
```

```python
# Finish the connection URI
connection_uri = "postgresql://repl:password@localhost:5432/dwh"
db_engine_dwh = sqlalchemy.create_engine(connection_uri)

# Transformation step, join with recommendations data
film_pdf_joined = film_pdf.join(recommendations)

# Finish the .to_sql() call to write to store.film. If the table exists, replace it completely.
film_pdf_joined.to_sql("film", db_engine_dwh, schema="store", if_exists="replace")

# Run the query to fetch the data
pd.read_sql("SELECT film_id, recommended_film_ids FROM store.film", db_engine_dwh)
```

## 3.4 Complete the ETL pipeline

We've now covered the full extent of an ETL pipeline. We've extracted data from databases, transformed the data to fit our needs, and loaded them back into a database, the data warehouse. This kind of batched ETL needs to run at a specific moment, and maybe after we completed some other tasks.

**The ETL function**

```python
def extract_table_to_df(tablename, db_engine):
    return pd.read_sql("SELECT * FROM {}".format(tablename), db_engine)

def split_columns_transform(df, column, pat, suffixes): 
# Converts column into str and splits it on pat...

def load_df_into_dwh(film_df, tablename, schema, db_engine):
    return pd.to_sql(tablename, db_engine, schema=schema, if_exists="replace")

db_engines = { ... } # Needs to be configured def etl():
    # Extract
    film_df = extract_table_to_df("film", db_engines["store"]) 
    # Transform
    film_df = split_columns_transform(film_df, "rental_rate", ".", ["_dollar", "_cents"]) 
    # Load
    load_df_into_dwh(film_df, "film", "store", db_engines["dwh"])
```

First of all, it's nice to have your ETL behavior encapsulated into a clean `etl()` function. Let's say we have a `extract_table_to_df()` function, which extracts a PostgreSQL table into a pandas DataFrame. Then we could have one or many transformation functions that takes a pandas DataFrame and transform it by putting the data in a more suitable format for analysis. This function could be called `split_columns_transform()`, for example. Last but not least, a `load_df_into_dwh()` function loads the transformed data into a PostgreSQL database. We can define the resulting `etl()` function as above. The result of `extract_table_to_df()` is used as an input for the transform function. We then use the output of the transform as input for `load_df_into_dwh`.

**Airflow refresher**

Now that we have a python function that describes the full ETL, we need to make sure that this function runs at a specific time. Before we go into the specifics, let's look at a small recap of Airflow [here](https://ledinhtrunghieu.github.io/2021/04/23/IntroductiontoDE.html#24-workflow-scheduling-frameworks)

**Scheduling with DAGs in Airflow**

So the first thing we need to do is to create the DAG itself. In this code sample, we keep it simple and create a DAG object with id `sample`. The second argument is `schedule_interval` and it defines when the DAG needs to run.

```python
from airflow.models import DAG
dag = DAG(dag_id="sample",
            ...,
            schedule_interval="0 0 * * *")
```

There are multiple ways of defining the interval, but the most common one is using a cron expression. That is a string which represents a set of times. It's a string containing 5 characters, separated by a space. The leftmost character describes minutes, then hours, day of the month, month, and lastly, day of the week. Going into detail would drive us too far, but there are several great resources to learn cron expressions online, for example, the website: [cron](https://crontab.guru). The DAG in the code sample run every 0th minute of the hour.

```
#	cron
#	.-------------------------	minute			(0	-	59)
#	| .-----------------------	hour			(0	-	23)
#	| | .---------------------	day of	the	month	(1	-	31)
#	| | | .-------------------	month			(1	-	12)
#	|	|	|	|	.-----------------	day of	the	week	(0	-	6)
#	*	*	*	*	* <command>						

# Example
0 * * * * # Every hour at the 0th minute
```

**The DAG definition file**

```python
from airflow.models import DAG
from airflow.operators.python_operator import PythonOperator

dag = DAG(dag_id="etl_pipeline",
          schedule_interval="0 0 * * *")

etl_task = PythonOperator(task_id="etl_task",
                          python_callable=etl, 
                          dag=dag)

etl_task.set_upstream(wait_for_this_task)
```

Having created the DAG, it's time to set the ETL into motion. The etl() function we defined earlier is a Python function, so it makes sense to use the PythonOperator function from the **python_operator** submodule of airflow. Looking at the documentation of the PythonOperator function, we can see that it expects a Python callable. In our case, this is the `etl()` function we defined before. It also expects two other parameters we're going to pass: `task_id` and `dag`. These are parameters which are standard for all operators. They define the identifier of this task, and the DAG it belongs to. We fill in the DAG we created earlier as a source. We can now set upstream or downstream dependencies between tasks using the `.set_upstream()` or `.set_downstream()` methods. By using `.set_upstream` in the example, `etl_task` will run after `wait_for_this_task` is completed.

Save as etl_dag.py 
Once you have this DAG definition and some tasks that relate to it, you can write it into a python file and place it in the DAG folder of Airflow. The service detects DAG and shows it in the interface.

<img src="/assets/images/20210423_IntroductiontoDE/pic30.png" class="largepic"/>

**Defining a DAG Example**

In the previous example you applied the three steps in the ETL process:
* Extract: Extract the film PostgreSQL table into pandas.
* Transform: Split the rental_rate column of the film DataFrame.
* Load: Load a the film DataFrame into a PostgreSQL data warehouse.
The functions extract_film_to_pandas(), transform_rental_rate() and load_dataframe_to_film() are defined in your workspace. In this example, you'll add an ETL task to an existing DAG. The DAG to extend and the task to wait for are defined in your workspace are defined as dag and wait_for_table respectively.

```python
# Define the ETL function
def etl():
    film_df = extract_film_to_pandas()
    film_df = transform_rental_rate(film_df)
    load_dataframe_to_film(film_df)

# Define the ETL task using PythonOperator
etl_task = PythonOperator(task_id='etl_film',
                          python_callable=etl,
                          dag=dag)

# Set the upstream to wait_for_table and sample run etl()
etl_task.set_upstream(wait_for_table)
etl()
```

**Setting up Airflow**

In this example, you'll learn how to add a DAG to Airflow. 

You'll need to move the dag.py file containing the DAG you defined in the previous exercise to, the DAGs folder. Here are the steps to find it:
* The airflow home directory is defined in the AIRFLOW_HOME environment variable. Type echo $AIRFLOW_HOME to find out.
* In this directory, find the airflow.cfg file. Use head to read the file, and find the value of the dags_folder.

Now you can find the folder and move the dag.py file there: mv ./dag.py <dags_folder>.

```
repl:~$ echo $AIRFLOW_HOME
~/airflow
repl:~$ cd ./airflow
repl:~/airflow$ ls
airflow.cfg  airflow.cfg.bak  airflow.db  airflow-webserver.pid  dags  logs  unittests.cfg
repl:~/airflow$ head airflow.cfg
[core]
# The home folder for airflow, default is ~/airflow
airflow_home = /home/repl/airflow

# The folder where your airflow pipelines live, most likely a
# subfolder in a code repository
# This path must be absolute
dags_folder = /home/repl/airflow/dags

# The folder where airflow should store its log files
repl:~/airflow$ cd ./dags
repl:~/airflow/dags$ ls
dag.py  dag_recommendations.py  __pycache__
```
# 4. Case Study: DataCamp


<img src="/assets/images/20210423_IntroductiontoDE/pic31.png" class="largepic"/>

**Querying the table**

```python
# Complete the connection URI
connection_uri = "postgresql://repl:password@localhost:5432/datacamp_application"
db_engine = sqlalchemy.create_engine(connection_uri)

# Get user with id 4387
user1 = pd.read_sql("SELECT * FROM rating WHERE user_id = '4387'", db_engine)

# Get user with id 18163
user2 = pd.read_sql("SELECT * FROM rating WHERE user_id = '18163'", db_engine)

# Get user with id 8770
user3 = pd.read_sql("SELECT * FROM rating WHERE user_id = '8770'", db_engine)

# Use the helper function to compare the 3 users
print_user_comparison(user1, user2, user3)
```
<img src="/assets/images/20210423_IntroductiontoDE/pic32.png" class="largepic"/>

**Average rating per course**
```python
# Complete the transformation function
def transform_avg_rating(rating_data):
  # Group by course_id and extract average rating per course
  avg_rating = rating_data.groupby('course_id').rating.mean()
  # Return sorted average ratings per course
  sort_rating = avg_rating.sort_values(ascending=False).reset_index()
  return sort_rating

# Extract the rating data into a DataFrame    
rating_data = extract_rating_data(db_engines)

# Use transform_avg_rating on the extracted data and print results
avg_rating_data = transform_avg_rating(rating_data)
print(avg_rating_data) 
```
<img src="/assets/images/20210423_IntroductiontoDE/pic33.png" class="largepic"/>

**Filter out corrupt data**

```python
course_data = extract_course_data(db_engines)

# Print out the number of missing values per column
print(course_data.isnull().sum())

# The transformation should fill in the missing values
def transform_fill_programming_language(course_data):
    imputed = course_data.fillna({"programming_language": "r"})
    return imputed

transformed = transform_fill_programming_language(course_data)

# Print out the number of missing values per column of transformed
print(transformed.isnull().sum())
```
**Using the recommender transformation**

```python
# Complete the transformation function
def transform_recommendations(avg_course_ratings, courses_to_recommend):
    # Merge both DataFrames
    merged = courses_to_recommend.merge(avg_course_ratings)
    # Sort values by rating and group by user_id
    grouped = merged.sort_values("rating", ascending = False).groupby('user_id') 
    # Produce the top 3 values and sort by user_id
    recommendations = grouped.head(3).sort_values("user_id").reset_index()
    final_recommendations = recommendations[["user_id", "course_id","rating"]]
    # Return final recommendations
    return final_recommendations

# Use the function with the predefined DataFrame objects
recommendations = transform_recommendations(avg_course_ratings, courses_to_recommend)
```

**Scheduling **

Previous example:
* Extract using `extract_course_data()` and `extract_rating_data()`
* Clean up NA using `transform_fill_programming_language()`
* Average course ratings per course: `transform_avg_rating()`
* Get eligible user and course id pair: `transform_courses_to_recommend()`
* Calculate the recommendations: `transform_recommendations()`

Loading to Postgresql

* Use the calculations in data products 
* Update daily
* Example use case: sending out e-mails with recommendations

The loading phase

```
recommendations.to_sql( 
    "recommendations", 
    db_engine, 
    if_exists="append",
)
```

To get the data into the table, we can take the recommendations DataFrame and use the `pandas` `.to_sql` method to write it to a SQL table. It takes the table name as a first argument, a database engine, and finally, we can define a strategy for when the table exists. We could use `"append"` as a value, for example, if we'd like to add records to the database instead of replacing them

```python
def etl(db_engines):
    # Extract the data
    courses = extract_course_data(db_engines)
    rating = extract_rating_data(db_engines)
    # Clean up courses data
    courses = transofrm_fill_programming_language(courses)
    # Get the average course ratings
    avg_course_rating = transform_avg_rating(rating)
    # Get eligible user and course id pairs
    courses_to_recommend = transform_courses_to_recommend(
        rating,
        course,
    )
    # Calculate the recommendations
    recommendatiosn = trasform_recommendantions(
        avg_course_rating,
        courses_to_recommend,    
    )
    # Load the recommendations into the database
    load_to_dwh(recommendations,db_enginen)

```
**Creating the DAG**
```python
from airflow.models import DAG
from airflow.operators.python_operator import PythonOperator

dag = DAG(dag_id="recommendations", 
          scheduled_interval="0 0 * * *")
task_recommendations = PythonOperator( 
    task_id="recommendations_task",
    python_callable=etl,
)
```
The target table
```python
connection_uri = "postgresql://repl:password@localhost:5432/dwh"
db_engine = sqlalchemy.create_engine(connection_uri)

def load_to_dwh(recommendations):
    recommendations.to_sql("recommendations", db_engine, if_exists="replace")
```
Defining the DAG
```python
# Define the DAG so it runs on a daily basis
dag = DAG(dag_id="recommendations",
          schedule_interval="0 0 * * *")

# Make sure `etl()` is called in the operator. Pass the correct kwargs.
task_recommendations = PythonOperator(
    task_id="recommendations_task",
    python_callable=etl,
    op_kwargs={"db_engines":db_engines},
)
```
**Querying the recommendations**
```python
def recommendations_for_user(user_id, threshold=4.5):
  # Join with the courses table
  query = """
  SELECT title, rating FROM recommendations
    INNER JOIN courses ON courses.course_id = recommendations.course_id
    WHERE user_id=%(user_id)s AND rating>%(threshold)s
    ORDER BY rating DESC
  """
  # Add the threshold parameter
  predictions_df = pd.read_sql(query, db_engine, params = {"user_id": user_id, 
                                                           "threshold": threshold})
  return predictions_df.title.values

# Try the function you created
print(recommendations_for_user(12, 4.65))
```

# 5. Reference


1. [Introduction to Data Engineering - DataCamp](https://learn.datacamp.com/courses/introduction-to-data-engineering)

