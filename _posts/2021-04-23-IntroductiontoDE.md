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

Let's look into a more practical example. We're starting with a dataset of all Olympic events from 1896 until 2016. From this dataset, you want to get an average age of participants for each year. For this example, let's say you have four processing units at your disposal. You decide to distribute the load over all of your processing units. To do so, you need to split the task into smaller subtasks. In this example, the average age calculation for each group of years is as a subtask. You can achieve that through `groupby.` Then, you distribute all of these subtasks over the four processing units. This example illustrates roughly how the first distributed algorithms like Hadoop MapReduce work, the difference being the processing units are distributed over several machines.


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
We could use the `multiprocessing.Pool API` to distribute work over several cores on the same machine. We have a function `take_mean_age`, which accepts a tuple: the year of the group and the group itself as a DataFrame. `take_mean_age` returns a DataFrame with one observation and one column: the mean age of the group. 
The resulting DataFrame is indexed by year. We can then take this function, and map it over the groups generated by `.groupby()`, using the `.map()` method of `Pool`. By defining `4` as an argument to `Pool`, the mapping runs in 4 separate processes, and thus uses 4 cores. Finally, we can concatenate the results to form the resulting DataFrame.

**Dask**

Several packages offer a layer of abstraction to avoid having to write such low-level code. For example, the `dask` framework offers a DataFrame object, which performs a groupby and apply using multiprocessing out of the box. You need to define the number of partitions, for example, `4`. `dask` divides the DataFrame into 4 parts, and performs `.mean()` within each part separately. Because `dask` uses lazy evaluation, you need to add `.compute()` to the end of the chain.

```
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

```
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

```
import dask.dataframe as dd

# Set the number of partitions
athlete_events_dask = dd.from_pandas(athlete_events, npartitions = 4)

# Calculate the mean Age per Year
print(athlete_events_dask.groupby('Year').Age.mean().compute())
```

## 2.3 Parallel computing framework

**Hadoop**

<img src="/assets/images/20210501_IntroductiontoDE/pic12.png" class="largepic"/>

**Hadoop** is a collection of open source projects, maintained by the Apache Software Foundation. Some of them are a bit outdated, but it's still relevant to talk about them. There are two Hadoop projects we want to focus on: **MapReduce** and **HDFS**.

**HDFS**

<img src="/assets/images/20210501_IntroductiontoDE/pic13.png" class="largepic"/>

**HDFS** is a **Hadoop Distributed File System**. It's similar to the **file system** you have on your computer, the only difference being **the files reside on multiple different computers**. HDFS has been essential in the big data world, and for parallel computing by extension. Nowadays, cloud-managed storage systems like Amazon S3 often replace HDFS.

**MapReduce**

<img src="/assets/images/20210501_IntroductiontoDE/pic14.png" class="largepic"/>

**MapReduce** was one of the first popularized  **big-data processing paradigms**. It works similar to the previous example, where the program splits tasks into subtasks, distributing the workload and data between several processing units. For MapReduce, these processing units are several computers in the cluster. MapReduce had its flaws; one of it was that it was hard to write these MapReduce jobs. Many software programs popped up to address this problem, and one of them was Hive.

**Hive**

<img src="/assets/images/20210501_IntroductiontoDE/pic15.png" class="largepic"/>

**Hive** is a layer on top of the Hadoop ecosystem that makes data from several sources queryable in a structured way using Hive's SQL variant: Hive SQL. Facebook initially developed Hive, but the Apache Software Foundation now maintains the project. Although MapReduce was initially responsible for running the Hive jobs, it now integrates with several other data processing tools.

Hive Exmaple: 

```
SELECT year, AVG(age) 
FROM views.athlete_events 
GROUP BY year
```

<img src="/assets/images/20210501_IntroductiontoDE/pic16.png" class="largepic"/>

This Hive query selects the average age of the Olympians per Year they participated. As you'd expect, this query looks indistinguishable from a regular SQL query. However, behind the curtains, this query is transformed into a job that can operate on a cluster of computers.

**Spark**

**Spark** **distributes** data processing tasks between clusters of computers. While MapReduce-based systems tend to need expensive disk writes between jobs, Spark tries to **keep as much processing as possible in memory**. In that sense, Spark was also an answer to the limitations of MapReduce. The disk writes of MapReduce were especially limiting in interactive exploratory data analysis, where each step builds on top of a previous step. Spark originates from the University of California, where it was developed at the Berkeley's AMPLab. Currently, the Apache Software Foundation maintains the project.

**Resilient distributed datasets (RDD)**

Spark's architecture relies on something called **resilient distributed datasets**, or **RDDs**. Without diving into technicalities, this is a data structure that maintains data which is distributed between multiple nodes. Unlike DataFrames, RDDs don't have named columns. From a conceptual perspective, you can think of RDDs as lists of tuples. We can do two types of operations on these data structures: transformations, like map or filter, and actions, like count or first. Transformations result in transformed RDDs, while actions result in a single result.

**PySpark**

When working with Spark, people typically use a programming language interface like PySpark. PySpark is the Python interface to spark. There are interfaces to Spark in other languages, like R or Scala, as well. PySpark hosts a DataFrame abstraction, which means that you can do operations very similar to pandas DataFrames. PySpark and Spark take care of all the complex parallel computing operations.

Have a look at the following PySpark example. Similar to the Hive Query you saw before, it calculates the mean age of the Olympians, per Year of the Olympic event. Instead of using the SQL abstraction, like in the Hive Example, it uses the DataFrame abstraction.

```
# Load the dataset into athlete_events_spark first
(athlete_events_spark
.groupBy('Year')
.mean('Age')
.show())
```

simillar to

```
SELECT year, AVG(age) 
FROM views.athlete_events 
GROUP BY year
```

Compare Hadoop vs PySpark vs Hive

<img src="/assets/images/20210501_IntroductiontoDE/pic18.png" class="largepic"/>

**PySpark Example**

In this example, You'll use the PySpark package to handle a Spark DataFrame. The data is the same as in previous exercises: participants of Olympic events between 1896 and 2016.
The Spark Dataframe, athlete_events_spark is available in your workspace.

```
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

<img src="/assets/images/20210501_IntroductiontoDE/pic19.png" class="largepic"/>

You can write a Spark job that pulls data from a CSV file, filters out some corrupt records, and loads the data into a SQL database ready for analysis. However, you need to do this every day as new data is coming in to the CSV file. 

One option is to run the job each day manually: Doesn't scale well: Weekends?

There are simple tools that could solve this problem, like cron, the Linux tool. However, let's say you have one job for the CSV file and another job to pull in and clean the data from an API, and a third job that joins the data from the CSV and the API together. The third job depends on the first two jobs to finish. It quickly becomes apparent that we need a more holistic approach, and a simple tool like cron won't suffice. So, we ended up with dependencies between jobs.

**DAGs**

<img src="/assets/images/20210501_IntroductiontoDE/pic20.png" class="largepic"/>

**A DAG (Directed Acyclic Graphs)** is a set of nodes that are connected by directed edges. There are no cycles in the graph, which means that no path following the directed edges sees a specific node more than once. In the example on the slide, Job A needs to happen first, then Job B, which enables Job C and D and finally Job E. As you can see, it feels natural to represent this kind of workflow in a DAG. The jobs represented by the DAG can then run in a daily schedule, for example.

Tools for the jobs: Luigi, cron, Apache Airflow

**Apache Airlow**

<img src="/assets/images/20210501_IntroductiontoDE/pic21.png" class="largepic"/>

Airbnb created **Airflow** as an internal tool for workflow management. They open-sourced Airflow in 2015, and it later joined the Apache Software Foundation in 2016. They built Airflow around the concept of DAGs. Using Python, developers can create and test these DAGs that build up complex pipelines.

<img src="/assets/images/20210501_IntroductiontoDE/pic22.png" class="largepic"/>

The first job starts a Spark cluster. Once it's started, we can pull in customer and product data by running the ingest_customer_data and ingest_product_data jobs. Finally, we aggregate both tables using the enrich_customer_data job which runs after both ingest_customer_data and ingest_product_data complete.

```
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

<img src="/assets/images/20210501_IntroductiontoDE/pic23.png" class="largepic"/>


```
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




