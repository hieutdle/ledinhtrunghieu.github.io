---
layout: post
author: ledinhtrunghieu
title: Test Preparation for AWS Cloud Practioner
---


# 1. AWS Cloud Practioner Essentrials

## 1.1. Introduction
**What is cloud computing?**
On-demand delivery of IT resources and applications through the internet with pay-as-you-go pricing


**What is another name for on-premises deployment?**
Private cloud deployment

**How does the scale of cloud computing help you to save costs?**
The aggregated cloud usage from a large number of customers results in lower pay-as-you-go prices.

## 1.2. Compute in the Cloud

Amazon EC2 instance types:
* General purpose instances: Balances compute, memory, and networking resources
* Compute optimized instances: Offers high-performance processors, batch processing workload.
* Memory optimized instances : Ideal for high-performance databases
* Accelerated computing instances: use hardware accelerators, or coprocessors, to perform some functions more efficiently than is possible in software running on CPUs. These functions include floating-point number calculations, graphics processing, and data pattern matching.
* Storage optimized instances : Suitable for data warehousing applications. Designed for workloads that require high, sequential read and write access to large datasets on local storage.

Amazon EC2 pricing:
* On-demand Instances
* Amazon EC2 Savings Plans: Savings Plans for several compute services
* Reserved Instances 
* Spot Instances
* Dedicated Hosts

Amazon EC2 **Auto Scaling:**
* Minimum capacity is the number of Amazon EC2 instances that launch immediately after you have created the Auto Scaling group.
* The desired capacity at two Amazon EC2 instances even though your application needs a minimum of a single Amazon EC2 instance to run.
* The third configuration that you can set in an Auto Scaling group is the maximum capacity. For example, you might configure the Auto Scaling group to scale out in response to increased demand, but only to a maximum of four Amazon EC2 instances.

**Elastic Load Balancing** is the AWS service that automatically distributes incoming application traffic across multiple resources, such as Amazon EC2 instances. 

**Amazon Simple Notification Service (Amazon SNS)** is a publish/subscribe service. Using Amazon SNS topics, a publisher publishes messages to subscribers.


**Amazon Simple Queue Service (Amazon SQS)** is a message queuing service. 
Using Amazon SQS, you can send, store, and receive messages between software components, without losing messages or requiring other services to be available. In Amazon SQS, an application sends messages into a queue. A user or service retrieves a message from the queue, processes it, and then deletes it from the queue.


**AWS Lambda** is a service that lets you run code without needing to provision or manage servers. While using AWS Lambda, you pay only for the compute time that you consume. 

**Amazon Elastic Container Service (Amazon ECS)** is a highly scalable, high-performance container management system that enables you to run and scale containerized applications on AWS. 

**Amazon Elastic Kubernetes Service (Amazon EKS)** is a fully managed service that you can use to run Kubernetes on AWS. 

**AWS Fargate** is a serverless compute engine for containers. It works with both Amazon ECS and Amazon EKS. 

## 1.3. Global infrastructure and reliability

Select region:
* Compliance with data governance and legal requirements
* Proximity to your customers
* Available services within a Region
* Pricing


An **Availability Zone** is a single data center or a group of data centers within a Region. Availability Zones are located tens of miles apart from each other. This is close enough to have low latency (the time between when content requested and received) between Availability Zones. However, if a disaster occurs in one part of the Region, they are distant enough to reduce the chance that multiple Availability Zones are affected.

Which statement best describes an Availability Zone?
=> A single data center or group of data centers within a Region


An **edge location** is a site that **Amazon CloudFront** uses to store cached copies of your content closer to your customers for faster delivery.

Ways to interact with AWS services:
* **The AWS Management Console** is a web-based interface for accessing and managing AWS services.
* To save time when making API requests, you can use the **AWS Command Line Interface (AWS CLI)**. 
* **Software development kits (SDKs)**. SDKs make it easier for you to use AWS services through an API designed for your programming language or platform.



With **AWS Elastic Beanstalk**, you provide code and configuration settings, and Elastic Beanstalk deploys the resources necessary to perform the following tasks:
* Adjust capacity
* Load balancing
* Automatic scaling
* Application health monitoring
(Create app based on provided Platform)

With **AWS CloudFormation**, you can treat your infrastructure as code


**Amazon CloudFront** is a content delivery service. It uses a network of edge locations to cache content and deliver content to customers all over the world. When content is cached, it is stored locally as a copy. This content might be video files, photos, webpages, and so on.

**AWS Outposts**: Extend AWS infrastructure and services to your on-premises data center.


## 1.4. Networking
A networking service that you can use to establish boundaries around your AWS resources is Amazon Virtual Private Cloud (Amazon VPC).

Amazon VPC enables you to provision an isolated section of the AWS Cloud. In this isolated section, you can launch resources in a virtual network that you define. Within a virtual private cloud (VPC), you can organize your resources into subnets. A subnet is a section of a VPC that can contain resources such as Amazon EC2 instances.


An **internet gateway** is a connection between a VPC and the internet. 

To access private resources in a VPC, you can use a **virtual private gateway**. 

**AWS Direct Connect** is a service that enables you to establish a dedicated private connection between your data center and a VPC.  


**Public subnets** contain resources that need to be accessible by the public, such as an online store’s website.

**Private subnets** contain resources that should be accessible only through your private network, such as a database that contains customers’ personal information and order histories. 

**A network access control list (ACL)** is a virtual firewall that controls inbound and outbound traffic at the subnet level.
Network ACLs perform stateless packet filtering. They remember nothing and check packets that cross the subnet border each way: inbound and outbound. 


**A security group** is a virtual firewall that controls inbound and outbound traffic for an Amazon EC2 instance.
By default, a security group denies all inbound traffic and allows all outbound traffic. You can add custom rules to configure which traffic to allow or deny.


**Which statement best describes an AWS account’s default network access control list?**
It is stateless and allows all inbound and outbound traffic.

**Domain Name System (DNS)**
Suppose that AnyCompany has a website hosted in the AWS Cloud. Customers enter the web address into their browser, and they are able to access the website. This happens because of Domain Name System (DNS) resolution. DNS resolution involves a customer DNS resolver communicating with a company DNS server.
You can think of DNS as being the phone book of the internet. DNS resolution is the process of translating a domain name to an IP address. 


**Amazon Route 53** is a DNS web service. It gives developers and businesses a reliable way to route end users to internet applications hosted in AWS. 



## 1.5. Storage and Databases
An **instance store** provides temporary block-level storage for an Amazon EC2 instance. An instance store is disk storage that is physically attached to the host computer for an EC2 instance, and therefore has the same lifespan as the instance. When the instance is terminated, you lose any data in the instance store.

**Amazon Elastic Block Store (Amazon EBS)** is a service that provides block-level storage volumes that you can use with Amazon EC2 instances. If you stop or terminate an Amazon EC2 instance, all the data on the attached EBS volume remains available.

**Amazon Simple Storage Service (Amazon S3)** is a service that provides object-level storage. Amazon S3 stores data as objects in buckets.

S3 storage class:
1. S3 Standard
2. S3 Standard-Infrequent Access (S3 Standard-IA). Similar to S3 Standard but has a lower storage price and higher retrieval price
3. S3 One Zone-Infrequent Access (S3 One Zone-IA). Stores data in a single Availability Zone  compared to S3 Standard and S3 Standard-IA, which store data in a minimum of three Availability Zones. Has a lower storage price than S3 Standard-IA. 
4. S3 Intelligent-Tiering. Ideal for data with unknown or changing access patterns. Requires a small monthly monitoring and automation fee per object.  If you haven’t accessed an object for 30 consecutive days, Amazon S3 automatically moves it to the infrequent access tier, S3 Standard-IA. If you access an object in the infrequent access tier, Amazon S3 automatically moves it to the frequent access tier, S3 Standard.
5. S3 Glacier: Low-cost storage designed for data archiving. Able to retrieve objects within a few minutes to hours
6. S3 Glacier Deep Archive: Lowest-cost object storage class ideal for archiving. Able to retrieve objects within 12 hours.


Amazon EBS vs Amazon S3:

Object storage treats any file as a complete, discreet object. Now this is great for documents, and images, and video files that get uploaded and consumed as entire objects, but every time there's a change to the object, you must re-upload the entire file. There are no delta updates. Block storage breaks those files down to small component parts or blocks. This means, for that 80-gigabyte file, when you make an edit to one scene in the film and save that change, the engine only updates the blocks where those bits live. If you're making a bunch of micro edits, using EBS, elastic block storage, is the perfect use case. If you were using S3, every time you saved the changes, the system would have to upload all 80 gigabytes, the whole thing, every time. If you are using complete objects or only occasional changes, S3 is victorious. If you are doing complex read, write, change functions, then, absolutely, EBS is your knockout winner.


**Amazon Elastic File System (Amazon EFS)** is a scalable file system used with AWS Cloud services and on-premises resources

EBS vs EFS:
An Amazon EBS volume stores data in a single Availability Zone.  To attach an Amazon EC2 instance to an EBS volume, both the Amazon EC2 instance and the EBS volume must reside within the same Availability Zone.
Amazon EFS is a regional service. It stores data in and across multiple Availability Zones. The duplicate storage
enables you to access data concurrently from all the Availability Zones in the Region where a file system is located.
Additionally, on-premises servers can access Amazon EFS using AWS Direct Connect.

EFS can be mounted on more than one EC2 instance at the same time, enabling access to files on EFS at the same time. EFS x10 price than EBS. Can be accessed by multiple EC2 instances simultaneously


S3 is An object store (not a file system).You can store files and "folders" but can't have locks, permissions etc like you would with a traditional file system

S3 is a storage facility accessible any where
EBS is a device you can mount onto EC2
EFS is a file system you can mount onto several EC2 instances at the same time

Money also important EFS > EBS > S3

EBS and EFS both outperform Amazon S3 in terms of IOPS (Input/output operations per second) and latency.

With a single API call, EBS can be scaled up or down. You can use EBS for database backups and other low-latency interactive applications that need reliable, predictable performance because it is less expensive than EFS.

Large amounts of data, such as large analytic workloads, are better served by EFS. Users must break up data and distribute it between EBS instances because data at this scale cannot be stored on a single EC2 instance allowed in EBS. The EFS service allows thousands of EC2 instances to be accessed at the same time, allowing vast volumes of data to be processed and analyzed in real-time.

**Amazon Relational Database Service (Amazon RDS)**
Amazon Relational Database Service (Amazon RDS) is a service that enables you to run relational databases in the AWS Cloud.


Amazon RDS is available on six database engines, which optimize for memory, performance, or input/output (I/O). Supported database engines include:
* Amazon Aurora
* PostgreSQL
* MySQL
* MariaDB
* Oracle Database
* Microsoft SQL Server

**Amazon Aurora** is an enterprise-class relational database. It is compatible with MySQL and PostgreSQL relational databases. It is up to five times faster than standard MySQL databases and up to three times faster than standard PostgreSQL databases.

**Amazon DynamoDB** is a key-value database service. It delivers single-digit millisecond performance at any scale. DynamoDB is serverless, which means that you do not have to provision, patch, or manage servers. As the size of your database shrinks or grows, DynamoDB automatically scales to adjust for changes in capacity while maintaining consistent performance. 


AWS RDS vs Amazon DynamoDB: Like Normal SQL vs NoSQL

**Amazon Redshift** is a data warehousing service that you can use for big data analytics. It offers the ability to collect data from many sources and helps you to understand relationships and trends across your data.

**AWS Database Migration Service (AWS DMS)** enables you to migrate relational databases, nonrelational databases, and other types of data stores.

Use cases for AWS DMS:
* Development and test database migrations: Enabling developers to test applications against production data without affecting production users
* Database consolidation: Combining several databases into a single database
* Continuous replication: Sending ongoing copies of your data to other target sources instead of doing a one-time migration

**Amazon DocumentDB** is a document database service that supports MongoDB workloads. (MongoDB is a document database program.)

**Amazon Neptune** is a graph database service. You can use Amazon Neptune to build and run applications that work with highly connected datasets, such as recommendation engines, fraud detection, and knowledge graphs.

**Amazon Quantum Ledger Database (Amazon QLDB)** is a ledger database service. You can use Amazon QLDB to review a complete history of all the changes that have been made to your application data. any entries can not remove from the audit. immutable.


**Amazon Managed Blockchain** is a service that you can use to create and manage blockchain networks with open-source frameworks. Blockchain is a distributed ledger system that lets multiple parties run transactions and share data without a central authority.

**Amazon ElastiCache** is a service that adds caching layers on top of your databases to help improve the read times of common requests. It supports two types of data stores: Redis and Memcached.

**Amazon DynamoDB Accelerator (DAX)** is an in-memory cache for DynamoDB. It helps improve response times from single-digit milliseconds to microseconds.

## 1.6. Security
Customers are responsible for the security of everything that they create and put in the AWS Cloud. AWS is responsible for security of the cloud. AWS operates, manages, and controls the components at all layers of infrastructure. 

Suppose that your company has multiple AWS accounts. You can use AWS Organizations to consolidate and manage multiple AWS accounts within a central location.

When you create an organization, AWS Organizations automatically creates a root, which is the parent container for all the accounts in your organization. In AWS Organizations, you can centrally control permissions for the accounts in your organization by using service control policies (SCPs). SCPs enable you to place restrictions on the AWS services, resources, and individual API actions that users and roles in each account can access.


In **AWS Organizations**, you can apply service control policies (SCPs) to the organization root, an individual member account, or an OU. An SCP affects all IAM users, groups, and roles within an account, including the AWS account root user.

 

You can apply IAM policies to IAM users, groups, or roles. You cannot apply an IAM policy to the AWS account root user.



**AWS Artifact** is a service that provides on-demand access to AWS security and compliance reports and select online agreements. AWS Artifact consists of two main sections: AWS Artifact Agreements and AWS Artifact Reports.

Suppose that your company needs to sign an agreement with AWS regarding your use of certain types of information throughout AWS services. You can do this through **AWS Artifact Agreements**. 

suppose that a member of your company’s development team is building an application and needs more information about their responsibility for complying with certain regulatory standards. You can advise them to access this information in AWS Artifact Reports. AWS Artifact Reports provide compliance reports from third-party auditors. These auditors have tested and verified that AWS is compliant with a variety of global, regional, and industry-specific security standards and regulations.

The Customer Compliance Center contains resources to help you learn more about AWS compliance. 

**AWS Shield** is a service that protects applications against DDoS attacks. AWS Shield provides two levels of protection: Standard and Advanced.

**AWS Shield Standard** automatically protects all AWS customers at no cost. It protects your AWS resources from the most common, frequently occurring types of DDoS attacks. 

**AWS Shield Advanced** is a paid service that provides detailed attack diagnostics and the ability to detect and mitigate sophisticated DDoS attacks. It also integrates with other services such as Amazon CloudFront, Amazon Route 53, and Elastic Load Balancing. Additionally, you can integrate AWS Shield with AWS WAF by writing custom rules to mitigate complex DDoS attacks.


**AWS WAF** is a **web application firewall** that helps protect your web applications or APIs against common web exploits and bots that may affect availability, compromise security, or consume excessive resources.

Encryption at rest and encryption in transit. By at rest, we mean when your data is idle. It's just being stored and not moving. For example, server-side encryption at rest is enabled on all DynamoDB table data. And that helps prevent unauthorized access. DynamoDB's encryption at rest also integrates with **AWS KMS**, or **Key Management Service**, for managing the encryption key that is used to encrypt your tables.

We use secure sockets layer, or SSL connections to encrypt data, and we can use service certificates to validate, and authorize a client. This means that data is protected when passing between Redshift, and our client. And this functionality exists in numerous other AWS services such as SQS, S3, RDS, and many more. 

**Amazon Inspector** Inspector helps to improve security, and compliance of your AWS deployed applications by running an automated security assessment against your infrastructure. Specifically, it helps to check on deviations of security best practices, exposure of EC2 instances, vulnerabilities, and so forth.


**Amazon GuardDuty** It analyzes continuous streams of metadata generated from your account, and network activity found on AWS CloudTrail events, Amazon VPC Flow Logs, and DNS logs. It uses integrated threat intelligence such as known malicious IP addresses, anomaly detection, and machine learning to identify threats more accurately. The best part is that it runs independently from your other AWS services.

**AWS Key Management Service (AWS KMS)** enables you to perform encryption operations through the use of cryptographic keys. A cryptographic key is a random string of digits used for locking (encrypting) and unlocking (decrypting) data. You can use AWS KMS to create, manage, and use cryptographic keys. You can also control the use of keys across a wide range of services and in your applications.

**AWS WAF** is a web application firewall that lets you monitor network requests that come into your web applications. 


**Which statement best describes an IAM policy?**
A document that grants or denies permissions to AWS services and resources


**An employee requires temporary access to create several Amazon S3 buckets. Which option would be the best choice for this task?**
IAM role

**Which statement best describes the principle of least privilege?**
Granting only the permissions that are needed to perform specific tasks

**Which service helps protect your applications against distributed denial-of-service (DDoS) attacks?**

AWS Shield

Amazon GuardDuty is a service that provides intelligent threat detection for your AWS infrastructure and resources. It identifies threats by continuously monitoring the network activity and account behavior within your AWS environment.
Amazon Inspector checks applications for security vulnerabilities and deviations from security best practices, such as open access to Amazon EC2 instances and installations of vulnerable software versions.
AWS Artifact is a service that provides on-demand access to AWS security and compliance reports and select online agreements.

## 1.7. Monitoring and Analytics

**Amazon CloudWatch** is a web service that enables you to monitor and manage various metrics and configure alarm actions based on data from those metrics.

With CloudWatch, you can create **alarms** that automatically perform actions if the value of your metric has gone above or below a predefined threshold. 

The CloudWatch **dashboard** feature enables you to access all the metrics for your resources from a single location

**AWS CloudTrail** records API calls for your account. The recorded information includes the identity of the API caller, the time of the API call, the source IP address of the API caller, and more. 

Within CloudTrail, you can also enable **CloudTrail Insights**. This optional feature allows CloudTrail to automatically detect unusual API activities in your AWS account. 

**Which tasks can you perform using AWS CloudTrail? (Select TWO.)**
* Filter logs to assist with operational analysis and troubleshooting
* Track user activities and API requests throughout your AWS infrastructure

AWS has an automated advisor called **AWS Trusted Advisor**. This is a service that you can use in your AWS account that will evaluate your resources against five pillars. The pillars are cost optimization, performance, security, fault tolerance, and service limits. Trusted Advisor in real time runs through a series of checks for each pillar in your account, based on AWS best practices, and it compiles categorized items for you to look into, and you can view them directly in the AWS console. 

**Which actions can you perform using Amazon CloudWatch? (Select TWO.)**
* Monitor your resources’ utilization and performance
* Access metrics from a single dashboard

Receiving real-time recommendations for improving your AWS environment can be performed by **AWS Trusted Advisor**.
Comparing your infrastructure to AWS best practices in five categories can be performed by **AWS Trusted Advisor**.
Automatically detecting unusual account activity can be performed by **AWS CloudTrail**.

## 1.8. Pricing and Support

The **AWS Free Tier** enables you to begin using certain services without having to worry about incurring costs for the specified period. 
Three types of offers are available: 
* Always Free
* 12 Months Free
* Trials

**The AWS Pricing Calculator** lets you explore AWS services and create an estimate for the cost of your use cases on AWS.

**AWS Billing & Cost Management** dashboard to pay your AWS bill, monitor your usage, and analyze and control your costs.


**AWS Organizations** also provides the option for **consolidated billing**. 

In **AWS Budgets**, you can create budgets to plan your service usage, service costs, and instance reservations.


In **AWS Budgets**, you could set a custom budget to notify you when your usage has reached half of this amount ($100).

**AWS Cost Explorer** is a tool that enables you to visualize, understand, and manage your AWS costs and usage over time.

AWS offers four different Support plans to help you troubleshoot issues, lower costs, and efficiently use AWS services. 
* Basic aws trusted advisor, aws health dashboard
* Developer email access
* Business direct phone access, aws trusted advisor full
* Enterprise 15 min SLA, TAM (technical account management)

If your company has an Enterprise Support plan, the TAM is your primary point of contact at AWS. They provide guidance, architectural reviews, and ongoing communication with your company as you plan, deploy, and optimize your applications. 

**AWS Marketplace** is a digital catalog that includes thousands of software listings from independent software vendors. You can use AWS Marketplace to find, test, and buy software that runs on AWS.

**AWS Marketplace** offers products in several categories, such as Infrastructure Products, Business Applications, Data Products, and DevOps.

**AWS Cloud Adoption Framework (AWS CAF)** organizes guidance into six areas of focus, called Perspectives. Each Perspective addresses distinct responsibilities. The planning process helps the right people across the organization prepare for the changes ahead.

In general, the **Business, People, and Governance** Perspectives focus on business capabilities, whereas the **Platform, Security, and Operations** Perspectives focus on technical capabilities.


The Business Perspective ensures that IT aligns with business needs and that IT investments link to key business results. Common roles in the Business Perspective include: 
* Business managers
* Finance managers
* Budget owners
* Strategy stakeholders

The People Perspective supports development of an organization-wide change management strategy for successful cloud adoption. Common roles in the People Perspective include: 
* Human resources
* Staffing
* People managers

The Governance Perspective focuses on the skills and processes to align IT strategy with business strategy. Common roles in the Governance Perspective include: 
* Chief Information Officer (CIO)
* Program managers
* Enterprise architects
* Business analysts
* Portfolio managers

The Platform Perspective includes principles and patterns for implementing new solutions on the cloud, and migrating on-premises workloads to the cloud. Common roles in the Platform Perspective include: 
* Chief Technology Officer (CTO)
* IT managers
* Solutions architects

The Security Perspective ensures that the organization meets security objectives for visibility, auditability, control, and agility. Common roles in the Security Perspective include: 
* Chief Information Security Officer (CISO)
* IT security managers
* IT security analysts

The Operations Perspective helps you to enable, run, use, operate, and recover IT workloads to the level agreed upon with your business stakeholders. Common roles in the Operations Perspective include: 
* IT operations managers
* IT support managers

Migration strategies:
* Rehosting also known as “lift-and-shift” involves moving applications without changes. 
* Replatforming, also known as “lift, tinker, and shift,” involves making a few cloud optimizations to realize a tangible benefit. Optimization is achieved without changing the core architecture of the application.
* Refactoring (also known as re-architecting) involves reimagining how an application is architected and developed by using cloud-native features. Refactoring is driven by a strong business need to add features, scale, or performance that would otherwise be difficult to achieve in the application’s existing environment.
* Repurchasing involves moving from a traditional license to a software-as-a-service model. 
* Retaining consists of keeping applications that are critical for the business in the source environment. This might include applications that require major refactoring before they can be migrated, or, work that can be postponed until a later time.
* Retiring is the process of removing applications that are no longer needed.

**The AWS Snow Family** is a collection of physical devices that help to physically transport up to exabytes of data into and out of AWS. 

AWS Snow Family is composed of **AWS Snowcone** , **AWS Snowball**, and **AWS Snowmobile**. 

AWS Snowcone is a small, rugged, and secure edge computing and data transfer device. It features 2 CPUs, 4 GB of memory, and 8 TB of usable storage.

**Snowball Edge Storage Optimized** devices are well suited for large-scale data migrations and recurring transfer workflows, in addition to local computing with higher capacity needs. 
Storage: 80 TB of hard disk drive (HDD) capacity for block volumes and Amazon S3 compatible object storage, and 1 TB of SATA solid state drive (SSD) for block volumes. 
Compute: 40 vCPUs, and 80 GiB of memory to support Amazon EC2 sbe1 instances (equivalent to C5).

**Snowball Edge Compute Optimized** provides powerful computing resources for use cases such as machine learning, full motion video analysis, analytics, and local computing stacks. 
Storage: 42-TB usable HDD capacity for Amazon S3 compatible object storage or Amazon EBS compatible block volumes and 7.68 TB of usable NVMe SSD capacity for Amazon EBS compatible block volumes. 
Compute: 52 vCPUs, 208 GiB of memory, and an optional NVIDIA Tesla V100 GPU. Devices run Amazon EC2 sbe-c and sbe-g instances, which are equivalent to C5, M5a, G3, and P3 instances.

With AWS, serverless** refers to applications that don’t require you to provision, maintain, or administer servers. You don’t need to worry about fault tolerance or availability. AWS handles these capabilities for you.


AWS offers a variety of services powered by artificial intelligence (AI). For example, you can perform the following tasks:
* Convert speech to text with **Amazon Transcribe**.
* Discover patterns in text with **Amazon Comprehend**.
* Identify potentially fraudulent online activities with **Amazon Fraud Detector**.
* Build voice and text chatbots with **Amazon Lex**.

Traditional machine learning (ML) development is complex, expensive, time consuming, and error prone. AWS offers **Amazon SageMaker** to remove the difficult work from the process and empower you to build, train, and deploy ML models quickly.You can use ML to analyze data, solve complex problems, and predict outcomes before they happen.

The **AWS Well-Architected Framework** helps you understand how to design and operate reliable, secure, efficient, and cost-effective systems in the AWS Cloud. It provides a way for you to consistently measure your architecture against best practices and design principles and identify areas for improvement.
The Well-Architected Framework is based on five pillars: 
* Operational excellence
* Security
* Reliability
* Performance efficiency
* Cost optimization


Six advantages of cloud computing:
* Trade upfront expense for variable expense.
* Benefit from massive economies of scale.
* Stop guessing capacity.
* Increase speed and agility.
* Stop spending money running and maintaining data centers.
* Go global in minutes.

**Amazon Textract** is a machine learning service that automatically extracts text and data from scanned documents.

**Amazon Augmented AI (Amazon A2I)** provides built-in human review workflows for common machine learning use cases, such as content moderation and text extraction from documents. With Amazon A2I, you can also create your own workflows for machine learning models built on Amazon SageMaker or any other tools.

**Trusted Advisor includes an ever-expanding list of checks in the following five categories:**
* Cost Optimization – recommendations that can potentially save you money by highlighting unused resources and opportunities to reduce your bill.
* Security – identification of security settings that could make your AWS solution less secure.
* Fault Tolerance – recommendations that help increase the resiliency of your AWS solution by highlighting redundancy shortfalls, current service limits, and over-utilized resources.
* Performance – recommendations that can help to improve the speed and responsiveness of your applications.
* Service Limits – recommendations that will tell you when service usage is more than 80% of the service limit.


