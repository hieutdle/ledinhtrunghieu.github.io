---
layout: post
author: ledinhtrunghieu
title: AWS Cloud Practioner Exam Practice
---


**Which of the following AWS services provides a security management tool that can be used to configure your AWS WAF rules across different AWS accounts?**
**AWS Firewall Manager**: AWS Firewall Manager is a security management service that allows you to centrally configure and manage firewall rules across your accounts and applications in AWS Organizations.

**AWS Resource Access Manager** is used to securely share your resources across your AWS accounts.

**A startup is using only an AWS Basic Support plan and cannot afford a higher plan right now. They require technical assistance from AWS to better understand the behavior of their services.**
**AWS Discussion Forums**. AWS Trusted Advisor is incorrect because you can’t receive technical assistance from this service. AWS Technical Account Manager is incorrect because you can’t get assistance from AWS TAM with just a basic support plan. AWS Concierge Support is incorrect. This offering is only available for enterprise support plans.

**A gaming company needs a service that uses the AWS global network to optimize users’ access speed to their applications through an anycast static IP address. Which of the following services fits this criteria?**
**AWS Global Accelerator** is a service that improves the availability and performance of your applications with local or global users. It provides you with static IP addresses that serve as a fixed entry point to your applications hosted in one or more AWS Regions. These IP addresses are anycast from AWS edge locations, so they’re announced from multiple AWS edge locations at the same time. This enables traffic to ingress onto the AWS global network as close to your users as possible.
**Amazon ElastiCache** is incorrect because it cannot route user traffic to the optimal endpoint. ElastiCache is primarily used to improve web applications’ performance by allowing you to retrieve information from a fast, managed, in-memory system, instead of relying entirely on slower disk-based databases.

**A developer needs to access a Linux EC2 Instance to modify a WordPress configuration file. Which of the following methods let them connect to their instance’s Linux terminal? (Select TWO.)**
To connect to an EC2 instance you can use:
* **Secure Shell (SSH)** – the most common tool to connect to Linux servers.
* **Session Manager** – it is a fully managed AWS Systems Manager capability that lets you manage your EC2 instances, on-premises instances, and virtual machines (VMs) through an interactive one-click browser-based shell or through the AWS CLI.
* **EC2 Instance Connect** – connect to your Linux instances using a browser-based client.

**A company plans to use an application streaming service to give its employees instant access to their desktop applications from any device.**
**Amazon AppStream 2.0** is a fully managed application streaming service that provides users with instant access to their desktop applications from anywhere. AppStream 2.0 manages the AWS resources required to host and run your applications, scales automatically, and provides access to your users on demand. AppStream 2.0 provides users access to the applications they need on the device of their choice, with a responsive, fluid user experience that is indistinguishable from natively installed applications.

**Amazon Kinesis Data Streams** is incorrect because this service is used to collect streaming data for real-time analytics.

**A customer needs to organize and consolidate information based on criteria specified in tags or resources in AWS. Which of the following services would you recommend to satisfy this requirement?**
**AWS Resource Groups** lets you organize AWS resources such as Amazon EC2 instances, Amazon Relational Database Service databases, and Amazon S3 buckets into groups using criteria that you define as tags. A resource group is a collection of resources that match the resource types specified in a query and share one or more tags or portions of tags. You can create a group of resources based on their roles in your cloud infrastructure, lifecycle stages, regions, application layers, or virtually any criteria.

# 1. Test 1

**What service provides the lowest-cost storage option for retaining database backups which also allows occasional data retrieval in minutes?**
**Amazon S3 Glacier** storage classes are designed to be the lowest-cost Amazon S3 storage classes, allowing you to archive large amounts of data at a very low cost

**Which of the following is a key financial benefit of migrating systems hosted on your on-premises data center to AWS?**
Opportunity to replace upfront capital expenses (CAPEX) with low variable costs.

**Which of the following Cost Management Tools allows you to track your Amazon EC2 Reserved Instance (RI) usage and view the discounted RI rate that was charged to your resources?**
**The Cost and Usage Report** is your one-stop shop for accessing the most granular data about your AWS costs and usage. You can also load your cost and usage information into Amazon Athena, Amazon Redshift, AWS QuickSight, or a tool of your choice.
**AWS Cost Explorer** is incorrect because this is just a tool that enables you to view and analyze your costs and usage but not at a granular level like the AWS Cost and Usage report. It also does not provide a way to load your cost and usage information into Amazon Athena, Amazon Redshift, AWS QuickSight, or a tool of your choice.


**A company is in the process of choosing the most suitable AWS Region to migrate its applications. Which of the following factors should they consider? (Select TWO.)**
Enhance customer experiences by reducing latency to users.
Support country-specific data sovereignty compliance requirements.
Please also read the answer carefully because you choose: Proximity to your end-users for **on-site visits** to your on-premises data center. ???

**Which of the following is an advantage of using managed services like RDS, ElastiCache, and CloudSearch in AWS?**
Simplifies all of your OS patching and backup activities to help keep your resources current and secure
Frees up the customer from the task of choosing and optimizing the underlying instance type and size of the service is incorrect because customers can still choose and optimize the underlying instance being used for their RDS, ElastiCache, and CloudSearch service.

**Which of the following shares a collection of offerings to help you achieve specific business outcomes related to enterprise cloud adoption through paid engagements in several specialty practice areas?**
**AWS Professional Services** shares a collection of offerings to help you achieve specific outcomes related to enterprise cloud adoption. Each offering delivers a set of activities, best practices, and documentation reflecting our experience supporting hundreds of customers in their journey to the AWS Cloud.
**AWS Technical Account Manager** is incorrect because this is your designated technical point of contact who provides advocacy and guidance to help plan and build solutions using best practices,


**What is the minimum number of Availability Zones that you should set up for your Application Load Balancer in order to create a highly available architecture?**
2


**Which of the following is the most cost-effective AWS Support Plan to use if you need access to AWS Support API for programmatic case management?**
Business
Both Basic and Developer support plans are incorrect since these types do not have access to the AWS Support API. Enterprise support plan is incorrect because although this one has access to the AWS Support API, it is still more expensive compared with the Business plan

**Which of the following can you use to resolve the connection between your on-premises VPN and your AWS virtual private cloud? (Select TWO.)**
Virtual Private Gateway
Amazon Route 53

**Which of the following allows you to set coverage targets and receive alerts when your utilization drops below the threshold you define?**
AWS Budgets
AWS Budgets gives you the ability to set custom budgets that alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount.

Amazon CloudWatch Billing Alarm is incorrect. Although you can use this to monitor your estimated AWS charges, this service still does not allow you to set coverage targets and receive alerts when your utilization drops below the threshold you define.

**Which of the following are the things that Amazon CloudWatch Logs can accomplish? (Select TWO.)**
Record AWS Management Console actions and API calls.
Monitor application logs from Amazon EC2 Instances.s

**A customer is planning to migrate some of their web applications that are hosted on-premises to AWS. Which of the following is a benefit of using AWS over virtualized data centers?**
Lower variable costs and lower upfront costs.

**Which of the following is true regarding the Developer support plan in AWS? (Select TWO.)**
Limited access to the 7 Core Trusted Advisor checks
No access to the AWS Support API

**Which service provides event history of your AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services?**
AWS CloudTrail is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account. With CloudTrail, you can log, continuously monitor, and retain account activity related to actions across your AWS infrastructure.

**Amazon CloudWatch** is incorrect because this service is primarily used to collect monitoring and operational data in the form of logs, metrics, and events, providing you with a unified view of AWS resources, applications, and services that run on AWS and on-premises servers.



