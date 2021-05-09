---
layout: post
author: ledinhtrunghieu
title: Lesson 1 - Data Engineering for Everyone
---
    

# 1. Data Engineering Introduction


In this chapter, you’ll learn what data engineering is and why demand for them is increasing. You’ll then discover where data engineering sits in relation to the data science lifecycle, how data engineers differ from data scientists, and have an introduction to your first complete data pipeline.


## 1.1 Data Workflow


There are four general steps through which **data flows** within an organization:


<img src="/assets/images/20210422_DEForEveryone/pic1.png" class="largepic"/>


1. First, we **collect** and **ingest** data, from web traffic, surveys, or media consumption for example.
2. Data is stored in raw format. The next step is to **prepare** it, which includes "cleaning data", for instance finding missing or duplicate values, and converting data into a more organized format.
3. Once the data is clean and organized, it can be **exploited**. We explore it, visualize it, build dashboards to track changes or compare two sets of data.
4. Finally, once we have a good grasp of our data, we're ready to **run experiments**, like evaluate which article title gets the most hits, or to build predictive models, for example to forecast stock prices.


## 1.2. Data Engineer's responsibilities


**Data engineers** are responsible for the first step of the process: ingesting collected data and storing it. Their job is to deliver the correct data, in the right form, to the right people, as efficiently as possible.

Data engineers:
* Ingest data from different sources
* Optimize the databases for analysis
* Remove corrupted data (manage data corruption)
* Develop, construct, test, and maintain architectures such as databases and large-scale processing systems to process and handle massive amounts of data. 


## 1.3. Big Data


The demand for data engineers has increased. **Big data** can be defined as:
* Data so large you have to think about how to deal with its size
* Difficult to process using traditional data management methods.

**Big data** is commonly characterized by **five Vs**: 
* Volume (the quantity of data points) (how much)
* Variety (type and nature of the data: text, image, video, audio) (what kind)
* Velocity (how fast the data is generated and processed) (how frequently)
* Veracity (how trustworthy the sources are) (how accurate)
* Value (how actionable the data is). (how useful)

Data engineers need to take all of this into consideration.


## 1.4. Data Engineer and Data Scientist


Data engineers lay the groundwork that makes data science activity possible.

**Data Engineer**:                                   
* Ingest and store data                              
* Set up databases
* Build data pipelines
* Strong software skills

**Data Scientist**:
* Exploit data
* Access databases
* Use pipeline outputs
* Strong analytical skills


## 1.5. Data Pipeline


You may have heard that "Data is the new oil", as first coined by The Economist, so let's follow this idea.

<img src="/assets/images/20210422_DEForEveryone/pic2.png" class="largepic"/>

We extract crude oil from an oil field. We move the crude oil to a distillation unit, where we separate the oil into several products.

<img src="/assets/images/20210422_DEForEveryone/pic3.png" class="largepic"/>

Some products are sent directly to their final users:
* Some pipes go straight to airports to deliver kerosene. 
* Gasoline are sent to gas storage facilities and stored in big tanks, before being distributed to gas stations. 
* Naphtha go through several chemical transformations.

<img src="/assets/images/20210422_DEForEveryone/pic4.png" class="largepic"/>

Companies ingest data from many different sources, which needs to be processed and stored in various ways. To handle that, we need **data pipelines** that efficiently automate the flow from one station to the next, so that data scientists can use up-to-date, accurate, relevant data.


At Spotflix, we have sources from which we extract data. For example, the users' actions and listening history on the mobile Spotflix app, the desktop Spotflix app and the Spotflix website itself.

The data is ingested into Spotflix's system, moving from their respective sources to our data lake. There are first six pipelines. We then organize the data, moving it from data lakes into different databases. We then use data pipelines again to split data to different places. There is one orange pipeline, which is data processing. After data processing, the data can then be stored in a new, clean tracks database that the scientist could use to buy some engine.

Database could be:
* Albums data, like label, producer, year of release,
* Tracks data, like name, length, featured artists, and number of listens,
* Playlists data, like name, song it contains, and date of creation,
* Customers data, like username, account opening date, subscription tier,
* Employees data, like name, salary, reporting manager, updated by human resources. 

Some albums data can be extracted and stored directly. For example, album cover pictures all have the same format, so we can store them directly without having to crop them. Employees could be split in different tables by department, for example sales, engineering, support,...

In a nutshell, **data pipelines** ensure the data flows efficiently through the organization. They automate extracting, transforming, combining, validating, and loading data, to reduce human intervention and errors, and decrease the time it takes for data to flow through the organization.


## 1.6. Introduction to ETL


It's a popular framework for designing data pipelines. It breaks up the flow of data into three sequential steps: 
* **E** for **extracting** the data
* **T** for **transforming** the data
* **L** for **loading** this transformed data to a new database.
* The key here is that data is processed before it's stored. 

In general, data pipelines move data from one system to another. They may follow ETL, but not all the time. For instance, the data may not be transformed, and routed directly to applications like visualization tools or Salesforce.



# 2. Storing Data


It’s time to talk about data storage—one of the main responsibilities for a data engineer. In this chapter, you’ll learn how data engineers manage different data structures, work in SQL—the programming language of choice for querying and storing data, and implement appropriate data storage solutions with data lakes and data warehouses.


## 2.1. Data Structures


**Structured data**: 
* Easy to search and organize.
* Consistent model: Data is entered following a rigid structure like a spreadsheet, rows and columns (tabular format)
* Define types: each column takes values of a certain type, like text, data, or decimal 
* Can be easily grouped to form relations
* Stored in relational databases
* About 20% of the data is structured
* Created and queried using SQL (Structured Query Language)

Because it's structured we can easily relate this table to other structured data. If there's another table holding the same information, we can connect to it using the number column. Tables that can be connected that way form a relational database.

**Semi-structured data**: 
* Semi-structured data resembles structured data, but allows more freedom
* Relatively easy to search and organize
* Pretty structured, but allows more flexibility
* Consistent model, less-rigid implementation: different observations have different sizes, different types
* Can be grouped, but needs more work NoSQL databases: usually leverages the JSON, XML, YAM file formats.

Favorite Artist JSON file:

<img src="/assets/images/20210422_DEForEveryone/pic6.png" class="largepic"/>

The model is consistent: each user id contains the user's last and first name, and their favorite artists. However, the number of favorite artists may differ: I have four, Sara has two and Lis has three favorite artists. Relational databases don't allow that kind of flexibility, but semi-structured formats let you do it.

**Unstructured data**:
* Does not follow a model, can't be contained in rows and columns
* Difficult to search and organize
* Usually text, sound, pictures or videos
* Usually stored in data lakes, can appear in data warehouses or databases
* Most of the data is unstructured
* Can be extremely valuable

We can use AI to search and organize unstructured data or add information to make structured data semi-structured.


## 2.2. SQL Databases Introduction


**SQL**
* Structured Query Language
* Industry standard for Relational Database Management System (RDBMS) (Systems that gather several tables, where all tables are related to each other)
* Allows you to access many records at once, and group, filter or aggregate them 
* Close to written English, easy to write and understand
* Data engineers use SQL to create and maintain databases
* Data scientists use SQL to query (request information from) databases

**Database schema**: Databases are made of many tables. The database schema governs how tables are related.


## 2.3. Data Warehouses, Data Lakes and Data Catalog


**Data Lakes**
* Stores all the collected raw data, just as it was uploaded from the different sources 
* It's unprocessed and messy
* Can be petabytes (1 million GBs)
* Store any kind of data or all data structures. 
* Cost-effective
* Difficulty to analyze
* Requires an up-to-date data catalog 
* Used by data scientists
* Big data, real-time analytics

**Data Warehouses**
* Specific data for specific use
* Relatively small
* Stores mainly structured data 
* More costly to update 
* Optimized for data analysis
* Also used by data analysts and business analysts
* Ad-hoc, read-only queries

**A data catalog** is a source of truth that compensates for the lack of structure in a data lake. It keeps track of 
* Where the data comes from
* How it is used
* Who is responsible for maintaining it
* How often it gets updated.
* Good practice in terms of data governance (managing the availability, usability, integrity and security of the data)
* Guarantees the reproducibility of the processes in case anything unexpected happens
* Necessary to prevent the data lake becoming a data swamp

It's good practice to have a data catalog referencing any data that moves through your organization:
* Reliability: We don't have to rely on tribal knowledge
* Autonomy: Makes us autonomous
* Scalability: Makes working with the data more scalable
* Speed: We can go from finding data to preparing it without having to rely on a human source of information every time we have a question.

**Database vs Data Warehouses**

Database: 
* Very general term
* Loosely defined as organized data stored and accessed on a computer

Data warehouse is a type of database


# 3. Moving and Processing Data


## 3.1. Data Processing


**Data processing** is converting raw data into meaningful information:
* Remove unwanted data: we don't need some data anymore after processed it.
* To save memory: Storing and processing data is not free. Uncompressed data can be ten times larger than compressed one
* Convert data from one type to another: Some data may come in a type, but would be easier to use in another
* Organize data:  The data is again processed to extract the metadata and store it in a database, for easy access by data analysts and data scientists
* Fit into a schema/structure: gather  data and fit it to the specific table schema using logic
* Increase productivity: automate all the data preparation steps we can, so that when it arrives to data scientists, they can analyze it almost immediately

**How data engineers process data**:
* Data manipulation, cleaning, and tidying tasks (What should we do when the genre is missing? Do we reject the file, do we leave the genre blank, or do we provide one by default?) that can be automated or that will always need to be done
* Store data in a sanely structured database
* Create views on top of the database tables
* Optimizing the performance of the database


## 3.2. Views


Data Engineer also ensure that the data is stored in a sanely structured database, and create **views** on top of the database tables for easy access by analysts.

**Views** are the output of a stored query on the data. For example, artist data and album data should be stored in separate tables in the database, but people will often want to work on these things together. That means data engineers need to create a view in the database combining both tables. Data engineers also optimize the performance of databases, for example by indexing the data so it's easier to retrieve.


## 3.3. Data Scheduling


**Scheduling**: 
* Can apply to any task listed in data processing
* Scheduling is the glue of your system
* Holds each piece and organize how they work together
* Runs tasks in a specific order and resolves all dependencies

There are different ways to glue things together:
* **Manually**: Manually update the employee table
* **Time**: Automatically run at a specific time: Update the employee table at 6 AM
* **Sensor scheduling**: Automatically run if a specific condition is met: Update the department tables if a new employee was added 

Sensor scheduling sounds like the best option but it requires having sensor always listening to see if somethings been added. This requires more resources and may not be worth it in this case. Manual and automated systems can also also work together: if a user manually upgrades their subscription tier on the app, automated tasks need to propagate this information to other parts of the system, to unlock new features and update billing information.

Software: Apache Spark

<img src="/assets/images/20210422_DEForEveryone/pic8.png" class="largepic"/>

**Batches and streams**

**Batches**:
* Group records at specific intervals
* Often cheaper because you can schedule it when resources aren't being used elsewhere, typically overnight

**Streams**:
* Individual data records are sent through the pipeline as soon as they are update
* Example of batch vs stream processing would be offline vs online listening. If a user listens online, Spotflix can stream parts of the song one after the other. If the user wants to save the song to listen offline, we need to batch all parts of the song together so they can save it

There's a third option called real-time, used for example in fraud detection, but for the sake of simplification and because streaming is almost always real-time, we will consider them to be the same in this blog.

Software: Apache Airflow and Luigi

<img src="/assets/images/20210422_DEForEveryone/pic9.png" class="largepic"/>


## 3.4. Parallel computing


**Parallel computing** sometimes also called parallel processing:
* Forms the basis of almost all modern data processing tools
* Important mainly for memory concerns, but also for processing power

When big data processing tools perform a processing task: 
* Split it up into several smaller subtasks
* Subtasks are then distributed over several computers.

Benefits: 
* Extra processing power (more processing units, divide works into multiple units => faster)
* Reduced memory footprint (Instead of needing to load all of the data in one computer's memory, you can partition the data and load the subsets into memory of different computers. That means the memory footprint per computer is relatively small)

Risks:
* Moving data incurs a cost
* Communication time (splitting a task into subtasks and merging the results of the subtasks back into one final result requires some communication between processes). In other words, if you have 2 processing units, a task that takes a few hundred milliseconds might not be worth splitting up. Additionally, due to the overhead, the speed does not increase linearly. 
* This effect is also called parallel slowdown.


## 3.5. Cloud computing


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

**Multicloud**

You don't need to take all your cloud services from the same provider though. You can use several ones: it's called multicloud

Advantages:
* Reducing reliance a single vendor
* Optimizes costs 
* Local laws requiring certain data to be physically present within the country
* Militating against disasters

Disadvantages:
* Cloud providers try to lock in consumers, by integrating as many of their services as they can.
* Some services from one provider may not be compatible with services from another one
* Make managing security and governance harder.


# 4. Reference


1. [Data Engineering for Everyone - DataCamp](https://campus.datacamp.com/courses/data-engineering-for-everyone)
