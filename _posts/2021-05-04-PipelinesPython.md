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







# 5. Reference

1. [Building Data Engineering Pipelines in Python - DataCamp](https://learn.datacamp.com/courses/building-data-engineering-pipelines-in-python)
