+++
title = "Introduction"
date = 2020-05-14T00:38:32+07:00
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

In this workshop, you will learn how to deploy a Fullstack Next.js 14 application with API Routes on AWS EC2,
using DocumentDB as the database. This step-by-step guide will help you set up a complete AWS environment, including
**IAM**, **VPC**, **EC2**, and **DocumentDB**, as well as test and deploy the application efficiently while ensuring
security and scalability.

![Workshop Architecture](/images/workshop_architecture.png)

### 🎯 Objectives:

- Understand how to create and configure **EC2, DocumentDB, CloudFront, and IAM on AWS**.

- Deploy a Fullstack NextJS 14 application on EC2.

- Connect and use DocumentDB from EC2, including migrating data from MongoDB to DocumentDB.

- Learn how to configure CloudFront as a **reverse proxy** to accelerate API Routes of NextJS on EC2.

### ✍ Requirements:

- An AWS account with IAM access.

- A computer with an SSH client (such as Terminal or PuTTY).

### ⚙️ Workflow:

**1. Client sends a request**

- The user accesses the website.
- The request is sent to CloudFront, which acts as a reverse proxy to accelerate Next.js API Routes on EC2.

**2. CloudFront processes the request**

- If the data is cached, CloudFront returns it immediately without calling EC2.
- If there is no cache, CloudFront forwards the request to Amazon EC2.

**3. EC2 processes the request**

- If the request requires database access, EC2 connects to Amazon DocumentDB.

**4. Querying data from Amazon DocumentDB**

- DocumentDB is located in a **private subnet** for security and cannot be accessed directly from the internet.
- EC2 sends queries to the Primary DocumentDB in AZ1.
- If read-only data is needed, DocumentDB can retrieve it from a Replica in AZ2 to reduce the load on the Primary.

**5. Returning results to the client**

- DocumentDB returns the data to EC2.
- EC2 processes the request and sends the response to CloudFront.
- CloudFront can cache the response to serve it faster in the future.
- Finally, the response is sent to the client.

**6. Managing access with IAM**

- IAM controls access for users and services.
- IAM Users belong to an IAM Group and follow policies that restrict access to AWS resources such as EC2 and DocumentDB.