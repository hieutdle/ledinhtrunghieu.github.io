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

