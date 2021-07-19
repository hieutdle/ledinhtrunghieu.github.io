---
layout: post
author: ledinhtrunghieu
title: Lesson 12B - Building Data Engineering Pipelines in Python
---

# 1. Ingesting Data

Explain what a data platform is, how data ends up in it, and how data engineers structure its foundations. Be able to ingest data from a RESTful API into the data platform’s data lake using a self-written ingestion pipeline, made using Singer’s taps and targets.

## 1.1. Components of a data platform

**Democratizing data increases insights**

Many modern organizations are becoming aware of just how valuable the data that they collected is. Internally, the data is becoming more and more “democratized”:

<img src="/assets/images/20210501_OOPInPython/pic1.png" class="largepic"/>

It is being made accessible to almost anyone within the company, so that new insights can be generated. Also on the public-facing side, companies are making more and more data available to people, in the form of e.g. public APIs.

<img src="/assets/images/20210501_OOPInPython/pic2.png" class="largepic"/>

* **Genesis of data**: The genesis of the data is with the operational systems, such as streaming data collected from various Internet of Things devices or websession data from Google Analytics or some sales platform. This data has to be stored somewhere, so that it can be processed at later times. Nowadays, the scale of the data and velocity at which it flows has lead to the rise of what we call “the **data lake**”.
* **Operational data is stored in the landing zone**: The **data lake** comprises several systems, and is typically organized in several zones. The data that comes from the operational systems for example, ends up in what we call the **“landing zone”**. This zone forms the basis of truth, it is always there and is the unaltered version of the data as it was received. **The process of getting data into the data lake is called “ingestion”**.
* **Cleaned data prevents rework**: People build various kinds of services on top of this data lake, like predictive algorithms, and dashboards for A/B tests of marketing teams. Many of these services apply similar transformations to the data. To prevent duplication of common transformations, data from the landing zone gets “cleaned” and stored in the **clean zone**. 
* **The business layer provides most insights**: Finally, per use case some special transformations are applied to this clean data. For example, predicting which customers are likely to churn is a common business use case. You would apply a machine learning algorithm to a dataset composed of several cleaned datasets. This domain specific data is stored in the business layer.
* **Pipelines move data from one zone to another**: To move data from one zone to another, and transform it along the way, people build **data pipelines**. The word comes from the similarity of how liquids and gases flow through pipelines, in this case it’s just data that flows. The pipelines can be triggered by external events, like files being stored in a certain location, on a time schedule or even manually. Usually, the pipelines that handle data in large batches, are triggered on a schedule, like overnight. We call these pipelines Extract, Transform and Load pipelines, or ETL pipelines.


**The Data Catalog**
* The `catalog` contains two objects. As with any Python dictionary, you access the object by name: `catalog["diaper_reviews"]`.
* Do not forget to call the `.read()` method on that object.
* Call `type()` on an object to investigate its class.

## 1.2. Data ingestion with Singer

**Singer’s core concepts**
* To get data into your data lake, at some moment you need to ingest it. There are several ways to do so, but it is convenient if within an organizational unit, the process is standardized. That is the aim of **Singe**r as well: to be the “**open-source standard for writing scripts that move data**”.
* **Singer** is a **specification** that describes how data extraction scripts and data loading scripts should communicate using a standard JSON-based data format over `stdout`. 
* `JSON` is similar to Python dictionaries. And `stdout` is a standardized “location” to which programs write their output. 
* Because Singer is a **specification**, these **extraction scripts**, which are called “**taps**”, and the **loading scripts**, which are called “**targets**”, can be written in any programming language. And they can easily be mixed and matched to create small data pipelines that move data from one place to another.
<img src="/assets/images/20210501_OOPInPython/pic3.png" class="largepic"/>
* Taps and targets communicate using 3 kinds of messages, which are sent to and read from specific **streams**:
    * schema (metadata)
    * state (process metadata)
    * record (data)

A **stream** is a *named* virtual location to which you send messages, that can be picked up at a downstream location. We can use different streams to partition data based on the topic for example: error messages would go to an error stream and data from different database tables could go to different streams as well.

**Describing the data through its schema**

Imagine you would need to pass this set of data to a process.
With the Singer spec, you would first describe the data, by specifying its schema. The schema should be given as a valid “JSON schema”, which is another specification that allows you to annotate and even validate structured data
You specify the data type of each property or field. You could also impose constraints like stating that the age should be an integer value between 1 and 130, as we’ve done here, or that a phone-number should be in a certain format. 
The last two keys in this JSON object are the “$id” and “$schema”. They allow you to uniquely specify this schema within your organization and tells others which version of JSON schema is being used. 
```py
columns = ("id", "name", "age", "has_children")
users = {(1, "Adrian", 32, False),
         (2, "Ruanne", 28, False),
         (3, "Hillary", 29, True)}
json_schema = {
    "properties": {"age": {"maximum": 130,
                           "minimum": 1,
                           "type": "integer"},

                   "has_children": {"type": "boolean"},
                   "id": {"type": "integer"},
                   "name": {"type": "string"}},

    "$id": "http://yourdomain.com/schemas/my_user_schema.json",
    "$schema": "http://json-schema.org/draft-07/schema#"}
```

You can tell the Singer library to make a **SCHEMA** message out of this JSON schema.
```py
import singer
singer.write_schema(schema=json_schema,
                    stream_name='DC_employees',
                    key_properties=["id"])
```

* You would call Singer’s “write_schema” function, passing it the “json_schema” we defined earlier. 
* With “stream_name” you specify the name of the stream this message belongs to. This can be anything you want. Data that belongs together, should be sent to the same stream. 
* The “key_properties” attribute should equal a list of strings that make up the primary key for records from this stream. 

<img src="/assets/images/20210501_OOPInPython/pic4.png" class="largepic"/>

The “write_schema” call simply wraps the actual JSON schema into a new JSON message and adds a few attributes.

**Serializing JSON**
```py
import json
json.dumps(json_schema["properties"]["age"])

'{"maximum": 130, "minimum": 1, "type": "integer"}'
```
```py
with open("foo.json", mode="w") as fh:
    json.dump(obj=json_schema, fp=fh)	# writes the json-serialized object
                                        # to the open file handle
```
* JSON is a common format, not just in Singer, but in many other places. Python provides the json module to work with JSON. To get objects in your code serialized as JSON, you would call either “json.dumps()” or “json.dump()”. 
* The former simply transforms the object to a string, whereas the latter writes that same string to a file.

**Working with JSON**

```py
# Import json
import json

database_address = {
  "host": "10.0.0.5",
  "port": 8456
}

# Open the configuration file in writable mode
with open("database_config.json", "w") as fh:
  # Serialize the object in this file handle
  json.dump(obj=database_address, fp=fh)
```

**Specifying the schema of the data**
Example of the products for a particular shop:
```json
{'items': [{'brand': 'Huggies',
            'model': 'newborn',
            'price': 6.8,
            'currency': 'EUR',            
            'quantity': 40,
            'date': '2019-02-01',
            'countrycode': 'DE'            
            },
           {…}]
```
```py
# Complete the JSON schema
schema = {'properties': {
    'brand': {'type': 'string'},
    'model': {'type': 'string'},
    'price': {'type': 'number'},
    'currency': {'type': 'string'},
    'quantity': {'type': 'integer', 'minimum': 1},
    'date': {'type': 'string', 'format': 'date'}, 
    'countrycode': {'type': 'string', 'pattern': "^[A-Z]{2}$"},
    'store_name': {'type': 'string'}}}

# Write the schema
singer.write_schema(stream_name='products', schema=schema, key_properties=[])
```
## 1.3. Running an ingestion pipeline with Singer

To convert one such user into a Singer RECORD message, we’d call the “write_record” function
```py
columns = ("id", "name", "age", "has_children")
users = {(1, "Adrian", 32, False),
         (2, "Ruanne", 28, False),
         (3, "Hillary", 29, True)}
```

The “stream_name” would need to match the stream you specified earlier in a schema message. Otherwise, these records are ignored. 

```py
singer.write_record(stream_name="DC_employees",
                    record=dict(zip(columns, users.pop())))

```
This would be almost equivalent to nesting the actual record dictionary in another dictionary that has two more keys, being the **“type”** and the **“stream”.**
```json
{"type": "RECORD", "stream": "DC_employees", "record": {"id": 1, "name": "Adrian", "age": 32, "has_children": false}}
```

That can be done elegantly with the unpacking operator, which are these 2 asterisks here preceding the “fixed_dict”.
That unpacks a dictionary in another one, and can be used in function calls as well. 
```py
fixed_dict = {"type": "RECORD", "stream": "DC_employees"}
record_msg = {**fixed_dict, "record": dict(zip(columns, users.pop()))} 
print(json.dumps(record_msg))
```


**Chaining taps and targets**
When you would combine the “write_schema” and “write_record” functions, you would have a Python module that prints JSON objects to stdout.
If you also have a Singer target that can parse these messages, then you have a full ingestion pipeline
It can simply deal with many records compared to the single one of “write_record”. 

```py
# Module: my_tap.py import singer
singer.write_schema(stream_name="foo", schema=…)
singer.write_records(stream_name="foo", records=…)
```
Ingestion pipeline: **Pipe** the tap’s output into a Singer target, using the `|` sym bol(Linux & MacOS)

We’re introducing the “target-csv” module, which is available on the Python Package Index. Its goal is to create CSV files from the JSON lines. The CSV file will be made in the same directory where you run this command, unless you configure it otherwise by providing a configuration file. 
Nothing prevents you from running a tap by parsing the code with the Python interpreter like this, but you’ll typically find taps and targets properly packaged, so you could call them directly, like this.
```py
python my_tap.py | target-csv
python my_tap.py | target-csv --config userconfig.cfg 
my-packaged-tap | target-csv --config userconfig.cfg
```

**Modular ingestion pipelines**
Each tap or target is designed to do one thing very well. They are easily configured through config files. By working with a standardized intermediate format, you could easily swap out the “target-csv” for “target-google-sheets” or “target-postgresql” for example, which write their output to whole different systems. This means you don’t need to write a lot of code, just pick the taps and targets that match with your intended source and destination.
```py
my-packaged-tap | target-csv
my-packaged-tap | target-google-sheets
my-packaged-tap | target-postgresql --config conf.json
```

```py
tap-custom-google-scraper | target-postgresql --config headlines.json
```

**Keeping track with state messages**

STATE messages yet: They are typically used to keep track of state, which is the way something is at some moment in time. That something is typically some form of memory of the process.
<img src="/assets/images/20210501_OOPInPython/pic5.png" class="largepic"/>

For example that you must extract only new records from this database daily at noon, local time. 
The easiest way to do so, is to keep track of the highest encountered “last_updated_on” value and emit that as a state message at the end of a successful run of your tap. 
Then, you can reuse the same message at a later time to extract only those records that were updated after this old state. 

```py
singer.write_state(value={"max-last-updated-on": some_variable})
```
You emit these state messages using the “write_state” function. The only required attribute is the value, which can be any JSON serializable object. The value field is free form and only for use by the same tap.

Run this `tap-mydelta` on 2019-06-14 at 12:00:00.000+02:00 (2nd row wasn't yet present then):

```json
{"type": "STATE", "value": {"max-last-updated-on": "2019-06-14T10:05:12.000+02:00"}}
```

**Communicating with an API**
```json
{'apis': [{'description': 'list the shops available',
           'url': '<api_key>/diaper/api/v1.0/shops'},
          {'description': 'list the items available in shop',
           'url': '<api_key>/diaper/api/v1.0/items/<shop_name>'}]}
{'shops': ['Aldi', 'Kruidvat', 'Carrefour', 'Tesco', 'DM']}
{'items': [{'brand': 'Huggies',
                'countrycode': 'DE',
                'currency': 'EUR',
                'date': '2019-02-01',
                'model': 'newborn',
                'price': 6.8,
                'quantity': 40},
               {'brand': 'Huggies',
                'countrycode': 'AT',
                'currency': 'EUR',
                'date': '2019-02-01',
                'model': 'newborn',
                'price': 7.2,
                'quantity': 40}]}
```
endpoint = "http://localhost:5000"

```py
# Fill in the correct API key
api_key = "scientist007"

# Create the web API’s URL
authenticated_endpoint = "{}/{}".format(endpoint, api_key)

# Get the web API’s reply to the endpoint
api_response = requests.get(authenticated_endpoint).json()
pprint.pprint(api_response)

# Create the API’s endpoint for the shops
shops_endpoint = "{}/{}/{}/{}".format(endpoint, api_key, "diaper/api/v1.0", "shops")
shops = requests.get(shops_endpoint).json()
print(shops)

# Create the API’s endpoint for items of the shop starting with a "D"
items_of_specific_shop_URL = "{}/{}/{}/{}/{}".format(endpoint, api_key, "diaper/api/v1.0", "items", "DM")
products_of_shop = requests.get(items_of_specific_shop_URL).json()
pprint.pprint(products_of_shop)
```

**Streaming records**

```py
# Use the convenience function to query the API

# Retrieve the products of the shop called Tesco.
tesco_items = retrieve_products("Tesco")

singer.write_schema(stream_name="products", schema=schema,
                    key_properties=[])

# Write a single record to the stream, that adheres to the schema
singer.write_record(stream_name="products", 
                    record={**tesco_items[0], "store_name": "Tesco"})

for shop in requests.get(SHOPS_URL).json()["shops"]:
    # Write all of the records that you retrieve from the API
    singer.write_records(
      stream_name="products", # Use the same stream name that you used in the schema
      records=({**tesco_items[0], "store_name": "Tesco"}
               for item in retrieve_products(shop))
    )    
```

```json
<script.py> output:
    {"type": "SCHEMA", "stream": "products", "schema": {"properties": {"brand": {"type": "string"}, "model": {"type": "string"}, "price": {"type": "number"}, "currency": {"type": "string"}, "quantity": {"type": "integer", "minimum": 1}, "date": {"type": "string", "format": "date"}, "countrycode": {"type": "string", "pattern": "^[A-Z]{2}$"}, "store_name": {"type": "string"}}}, "key_properties": []}
    {"type": "RECORD", "stream": "products", "record": {"countrycode": "IE", "brand": "Pampers", "model": "3months", "price": 6.3, "currency": "EUR", "quantity": 35, "date": "2019-02-07", "store_name": "Tesco"}}
    {"type": "RECORD", "stream": "products", "record": {"countrycode": "IE", "brand": "Pampers", "model": "3months", "price": 6.3, "currency": "EUR", "quantity": 35, "date": "2019-02-07", "store_name": "Tesco"}}
```

**Chain taps and targets**
Your company’s data lake, which is file system based, is made available to you under /home/repl/workspace/mnt/data_lake. Your goal is to add a file to it, using the Singer tap we’ve been building over the last few exercises, `tap-marketing-api`, and an already existing Singer target, `target-csv`.

```bash
tap-marketing-api | target-csv --config ingest/data_lake.conf
```

# 2. Creating a data transformation pipeline with PySpark

Process data in the data lake in a structured way using PySpark

## 2.1 Basic introduction to PySpark

**Spark**
* A fast and general engine for large-scale data processing
* 4 libraries built on top of Spark core:
<img src="/assets/images/20210501_OOPInPython/pic17.png" class="largepic"/>
    * **Spark SQL** for manipulating mostly tabular data
    * **Spark Streaming** for manipulating streaming data 
    * **MLlib** for machine learning 
    *  **GraphX** for graph analysis
* API in several languages
    * Java, Scala, Python()

**When to use Spark**
* Data processing at scale: scales incredibly well to dataset sizes of billions of records by parallelizing its execution over multiple machines. 
* Interactive analytics: notebooks in which data scientists explore data interactively, validate hypotheses about the data quickly and drill deeper into the results.
* Machine learning

**Spark is not used for**
* When you have only little data
* When you have only simple operations

**Business case: finding the perfect diaper**
Find the perfect diaper based on:
* qualitative attributes e.g. comfort
* quantitative attributes e.g. price

Scraped data available:
* prices.csv : pricing details per model per store
* ratings.csv: user ratings per model


**Starting the Spark analytics engine**
```py
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()

prices = spark.read.options(header="true").csv("mnt/data_lake/landing/prices.csv")

prices.show()
```
<img src="/assets/images/20210501_OOPInPython/pic19.png" class="largepic"/>

**Spark Automatically inferred data types**
```py
from pprint import pprint
pprint(prices.dtypes)
```
<img src="/assets/images/20210501_OOPInPython/pic18.png" class="largepic"/>

**Enforcing a schema**
```py
schema = StructType([StructField("store", StringType(), nullable=False), 
                     StructField("countrycode", StringType(), nullable=False), 
                     StructField("brand", StringType(), nullable=False), 
                     StructField("price", FloatType(), nullable=False), 
                     StructField("currency", StringType(), nullable=True), 
                     StructField("quantity", IntegerType(), nullable=True), 
                     StructField("date", DateType(), nullable=False)])

prices = spark.read.options(header="true").schema(schema).csv("mnt/data_lake/landing/prices.csv")
print(prices.dtypes)
```
<img src="/assets/images/20210501_OOPInPython/pic20.png" class="largepic"/>

## 2.2. Cleaning data

**Reasons to clean data**
* Incorrect data types
* Invalid rows (Especially when parsing manually entered data, from sources such as Microsoft Excel, some rows simply contain bogus information.)
* Incom plete rows (Sometimes almost all fields in a row are valid, except one or two. These fields are not critical and can be left empty, sometimes they can be given a default value)
* Badly chosen placeholders (strings such as “N/A” or “Unknown”)

**We can automate data cleaning**

**Select data types**
<img src="/assets/images/20210501_OOPInPython/pic21.png" class="largepic"/>

**Badly formatted source data**
```
cat bad_data.csv	# prints the entire file on stdout
```
<img src="/assets/images/20210501_OOPInPython/pic22.png" class="largepic"/>

Spark’s default handling of bad source data

```py
prices = spark.read.options(header="true").csv('landing/prices.csv')

prices.show()
```
<img src="/assets/images/20210501_OOPInPython/pic23.png" class="largepic"/>


Spark makes an effort to incorporate the invalid row. That’s not what we want.

**Handle invalid rows**
```py
prices = (spark
          .read
          .options(header="true", mode="DROPMALFORMED")
          .csv('landing/prices.csv'))
```
<img src="/assets/images/20210501_OOPInPython/pic24.png" class="largepic"/>

**The signigicance of null**
<img src="/assets/images/20210501_OOPInPython/pic25.png" class="largepic"/>

Sometimes data isn’t malformed, but simply incomplete. In this example, the 2nd row is missing a country code and a quantity. Removing the row entirely is not ideal, as it still contains useful information. As you can see, Spark’s default way of handling this is to fill the blanks with “null”, which is a well-established way to express missing or unknown values.

```py

prices = (spark.read.options(header="true")
          .schema(schema)
          .csv('/landing/prices_with_incomplete_rows.csv'))
prices.show()
```
<img src="/assets/images/20210501_OOPInPython/pic26.png" class="largepic"/>

**Supplying default values for missing data**
```py
prices.fillna(25, subset=['quantity']).show()
```
<img src="/assets/images/20210501_OOPInPython/pic27.png" class="largepic"/>
You could instruct Spark to fill the missing data with specific values. For that, we use the “fillna” method, which optionally accepts a list of column names as input. Only those columns will be affected, as you can see.


**Badly chosen placeholders**
Example: contracts of employees
```py
employees = spark.read.options(header="true").schema(schema).csv('employees.csv')
```
<img src="/assets/images/20210501_OOPInPython/pic28.png" class="largepic"/>

People often put placeholders for fields they don’t know the value of. In this example, someone gave an unrealistic date for the end of Alice’s employment contract. Such placeholders are bad for analytics purposes. In most cases, it’s better to have unknown data simply represented by the “null” value. Many libraries have built-in functionality to deal with this appropriately.

**Conditionally replace values**
```py
from pyspark.sql.functions import col, when
from datetime import date, timedelta

one_year_from_now = date.today().replace(year=date.today().year + 1)
better_frame = employees.withColumn("end_date",
    when(col("end_date") > one_year_from_now, None).otherwise(col("end_date")))
better_frame.show()
```
<img src="/assets/images/20210501_OOPInPython/pic29.png" class="largepic"/>

Here, we replace the values that are illogical by using a condition in the “when()” function. When the condition is met, we replace the values with Python’s “None”, which in Spark gets translated to “null”. Otherwise, we leave the column unaltered.


# 3. Testing your data pipeline






# 4. Managing and orchestrating a workflow

## 4.1. Modern day workflow management

**Workflow remind**
A workflow is a **sequence of tasks** that are either scheduled to run or that **could be triggered** by the occurrence of an event. Workflows are typically used to orchestrate data processing pipelines.
<img src="/assets/images/20210501_OOPInPython/pic6.png" class="largepic"/>

**Scheduling with cron**
Oftentimes we have tasks that need to run on a schedule. Churn prediction algorithms might need to be run weekly for example. An often used software utility in such cases is “cron”.
Cron reads configuration files, known as “crontab” files. 
You tabulate tasks that you want to run at a specific time. Here’s an example. This crontab record would execute the process “log_my_activity” at a specific time, which, read from left to right
```bash
*/15 9-17 * * 1-3,5 log_my_activity
```
* */15: Every 15 minutes
* 9-17: Between officer hour
* *: Everyday of the moth
* *: Every month of the year
* 1-3.5: Mondays,Tuesdays,Wednesdays and Fridays


**Modern workflow managers:**
* Luigi (Spotify, 2011, Python-based)
* Azkaban (LinkedIn, 20009, Java-based)
* Airflow (Airbnb, 2015, Python-based)

**The airflow task:**
* An instances of an Operator class
    * Inherits from `BaseOperator` -> Must implement `execute()` method
* Performs a specific action (delegation):
    * `BashOperator` -> run bash command/script
    * `PythonOperator` -> run Python script
    * `SparkSubmitOperator` -> submit a Spark job with a cluster

**Apache Airflow fulfills modern engineering needs**
1. Create and visualize com plex workflows
<img src="/assets/images/20210501_OOPInPython/pic7.png" class="largepic"/>
2. Monitor and log workflows
Airflow can show us when certain tasks failed or how long each task took and plot that in clear charts.
<img src="/assets/images/20210501_OOPInPython/pic8.png" class="largepic"/>
3. Scales horizontally 
As we get more tasks to execute, we want our tool to work with multiple machines, rather than increasing the performance of one single machine
<img src="/assets/images/20210501_OOPInPython/pic9.png" class="largepic"/>

**The Directed Acyclic Graph (DAG)**
<img src="/assets/images/20210501_OOPInPython/pic10.png" class="largepic"/>
The central piece in an Airflow workflow is the DAG, which is an acronym for Directed Acyclic Graph: 
* A graph is a collection of nodes that are connected by edges. 
* The “directed” part in the acronym implies that there is a sense of direction between the nodes. The arrows on the edges indicate the direction. 
* The “acyclic” part simply means that when you traverse the directed graph, there is no way for you to circle back to the same node.
* The nodes are “operators”, each instance of which can be given a unique label, the task id. 
* Operators do something, like run a Python script, or schedule tasks with a cloud provider. 
* They’re triggered by a scheduler, but executed by an executor, which is typically a different process.


```py
from airflow import DAG

my_dag = DAG(
    dag_id="publish_logs",
    schedule_interval="* * * * *",
    start_date=datetime(2010, 1, 1)
)
```

**Expressing dependencies between operators**
```py
dag = DAG(…)
task1 = BashOperator(…)
task2 = PythonOperator(…)
task3 = PythonOperator(…)
task1.set_downstream(task2)
task3.set_upstream(task2)
#	equivalent, but shorter:
#	task1 >> task2
#	task3 << task2
#	Even clearer:
#	task1 >> task2 >> task3
```

<img src="/assets/images/20210501_OOPInPython/pic11.png" class="largepic"/>

**Specifying the DAG schedule**
```py
from datetime import datetime
from airflow import DAG

reporting_dag = DAG(
    dag_id="publish_EMEA_sales_report", 
    # Insert the cron expression
    schedule_interval="0 7 * * 1",
    start_date=datetime(2019, 11, 24),
    default_args={"owner": "sales"}
)
```

**Specifying operator dependencies**

<img src="/assets/images/20210501_OOPInPython/pic12.png" class="largepic"/>

```py
# Specify direction using verbose method
prepare_crust.set_downstream(apply_tomato_sauce)

tasks_with_tomato_sauce_parent = [add_cheese, add_ham, add_olives, add_mushroom]
for task in tasks_with_tomato_sauce_parent:
    # Specify direction using verbose method on relevant task
    apply_tomato_sauce.set_downstream(task)

# Specify direction using bitshift operator
tasks_with_tomato_sauce_parent >> bake_pizza

# Specify direction using verbose method
bake_pizza.set_upstream(prepare_oven)
```

## 4.2. Building a data pipeline with Airflow

**Airflow's BashOperator**
* Executes bash com mands
* Airflow adds logging, retry options and metrics over running this yourself

```py
from airflow.operators.bash_operator import BashOperator
bash_task = BashOperator(
            task_id='greet_world',
            dag=dag,
            bash_command='echo "Hello, world!"'
)
```

**Airflow’s PythonOperator**
* Executes Python callables
```py
from airflow.operators.python_operator import PythonOperator from my_library import my_magic_function
python_task = PythonOperator(
    dag=dag,
    task_id='perform_magic',
    python_callable=my_magic_function,
    op_kwargs={"snowflake": "*", "amount": 42}
)
```

**Running PySpark from Airflow**
* **BashOperator:**
```py
spark_master = (
    "spark://"
    "spark_standalone_cluster_ip"
    ":7077")

command = (
    "spark-submit "
    "--master {master} "
    "--py-files package1.zip "
    "/path/to/app.py"
).format(master=spark_master)
BashOperator(bash_command=command, …)
```
* **SSH Operator**
```py
from airflow.contrib.operators\
    .ssh_operator import SSHOperator

task = SSHOperator(
    task_id='ssh_spark_submit',
    dag=dag,
    command=command,
    ssh_conn_id='spark_master_ssh'
)
```
* SparkSubmitOperator
```py
from airflow.contrib.operators\
    .spark_submit_operator \
    import SparkSubmitOperator

spark_task = SparkSubmitOperator(
    task_id='spark_submit_id',
    dag=dag,
    application="/path/to/app.py",
    py_files="package1.zip",
    conn_id='spark_default'
)
```

**Preparing a DAG for daily pipelines**
```py
# Create a DAG object
dag = DAG(
  dag_id='optimize_diaper_purchases',
  default_args={
    # Don't email on failure
    'email_on_failure': False,
    # Specify when tasks should have started earliest
    'start_date': datetime(2019, 6, 25)
  },
  # Run the DAG daily
  schedule_interval='@daily')
```
**Scheduling bash scripts with Airflow**
```py
config = os.path.join(os.environ["AIRFLOW_HOME"], 
                      "scripts",
                      "configs", 
                      "data_lake.conf")

ingest = BashOperator(
  # Assign a descriptive id
  task_id="ingest_data", 
  # Complete the ingestion pipeline
  bash_command='tap-marketing-api | target-csv --config %s' % config,
  dag=dag)
```

**Scheduling Spark jobs with Airflow**
```py
# Import the SparkSubmitOperator.
from airflow.contrib.operators.spark_submit_operator import SparkSubmitOperator

# Set the path for entry_point by joining the AIRFLOW_HOME environment variable and scripts/clean_ratings.py.
# Set the path for dependency_path by joining the AIRFLOW_HOME environment variable and dependencies/pydiaper.zip.
entry_point = os.path.join(os.environ["AIRFLOW_HOME"], "scripts", "clean_ratings.py")
dependency_path = os.path.join(os.environ["AIRFLOW_HOME"], "dependencies", "pydiaper.zip")

# Complete the clean_data task by passing a reference to the file that starts the Spark job and the additional files the job will use.
with DAG('data_pipeline', start_date=datetime(2019, 6, 25),
         schedule_interval='@daily') as dag:
  	# Define task clean, running a cleaning job.
    clean_data = SparkSubmitOperator(
        application=entry_point, 
        py_files=dependency_path,
        task_id='clean_data',
        conn_id='spark_default')
```
**Scheduling the full data pipeline with Airflow**

```py
spark_args = {"py_files": dependency_path,
              "conn_id": "spark_default"}
# Define ingest, clean and transform job.
with dag:
    ingest = BashOperator(task_id='Ingest_data', bash_command='tap-marketing-api | target-csv --config %s' % config)
    clean = SparkSubmitOperator(application=clean_path, task_id='clean_data', **spark_args)
    insight = SparkSubmitOperator(application=transform_path, task_id='show_report', **spark_args)
    
    # set triggering sequence
    ingest >> clean >> insight
```

## 4.3. Deploying Airflow

**Installing and configuring Airflow**
```py
export AIRFLOW_HOME=~/airflow

pip install apache-airflow

airflow initdb
```

<img src="/assets/images/20210501_OOPInPython/pic13.png" class="largepic"/>
<img src="/assets/images/20210501_OOPInPython/pic14.png" class="largepic"/>

**Setting up for production**
* `dags`: place to store the dags (con gurable)
* `tests`: unit test the possible deployment, possibly ensure consistency across DAGs
* `plugins`: store custom operators and hooks
* `connections`,`pools`,`variables`: provide a location for various configuration files you can import into Airflow 
<img src="/assets/images/20210501_OOPInPython/pic15.png" class="largepic"/>


**Example Airflow deployment test**
```py
from airflow.models import DagBag

def test_dagbag_import():
    """Verify that Airflow will be able to import all DAGs in the repository.""" 
    dagbag = DagBag()
    number_of_failures = len(dagbag.import_errors)
    assert number_of_failures == 0, \
        "There should be no DAG failures. Got: %s" % dagbag.import_errors
```

We first import and instantiate the DagBag, which is the collection of all DAGs found in a folder. Once instantiated, it holds a dictionary of error messages for DAGs that had issues, like Python syntax errors or the presence of cycles. If our testing framework would fail on this test, our CI/CD pipeline could prevent automatic deployment.

**Transferring DAGs and plugins**
<img src="/assets/images/20210501_OOPInPython/pic16.png" class="largepic"/>
How do you get your DAGs uploaded to the server? 
* If you keep all the DAGs in the repository that contains the basic installation layout. This can be done simply by cloning the repository on the Airflow server.
* Alternatively, if you keep a DAG file and any dependencies close to the processing code in another repository, you simply copy the DAG file over to the server with a tool like “rsync” for example. Or you make use of packaged DAGs, which are zipped archives that promote better isolation between projects. You’ll still need to copy over the zip file to the server though. You could also have the Airflow server regularly syncing the DAGs folder with a repository of DAGs, where everyone writes to.

```py
default_args = {
    "owner": "squad-a",
    "depends_on_past": False,
    "start_date": datetime(2019, 7, 5),
    "email": ["foo@bar.com"],
    "email_on_failure": False,
    "email_on_retry": False,
    "retries": 1,
    "retry_delay": timedelta(minutes=5),
}

dag = DAG(
    "cleaning",
    default_args=default_args,
    user_defined_macros={"env": Variable.get("environment")},
    schedule_interval="0 5 */2 * *"
)


def say(what):
    print(what)


with dag:
    say_hello = BashOperator(task_id="say-hello", bash_command="echo Hello,")
    say_world = BashOperator(task_id="say-world", bash_command="echo World")
    shout = PythonOperator(task_id="shout",
                           python_callable=say,
                           op_kwargs={'what': '!'})

    say_hello >> say_world >> shout
```

# 5. Reference

1. [Building Data Engineering Pipelines in Python - DataCamp](https://learn.datacamp.com/courses/building-data-engineering-pipelines-in-python)
