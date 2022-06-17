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




