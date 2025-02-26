+++
title = "Deploying Next.js 14 Fullstack on AWS EC2"
date = 2025
weight = 1
chapter = false
+++

# Workshop: Deploying Next.js 14 Fullstack On AWS EC2 With CloudFront & DocumentDB

### Overview

In this workshop, you will learn how to deploy a Fullstack Next.js 14 application with API Routes on AWS, utilizing EC2,
DocumentDB, IAM, and CloudFront to optimize performance and cost efficiency

![Workshop Architecture](/images/workshop_architecture.png)

### Objectives:

- Understand how to create and configure EC2, DocumentDB, CloudFront, and IAM on AWS.
- Deploy a Fullstack Next.js 14 application on EC2.

- Connect and use DocumentDB from EC2, including migrating data from MongoDB to DocumentDB.

- Learn how to configure CloudFront as a reverse proxy to optimize API Routes performance on EC2.

### Requirements:

- An AWS account with IAM access.

- A computer with an SSH client (e.g., Terminal or PuTTY).

### Contents

1. [Introduction](1-introduce/)
2. [Restrict Access with IAM Service](2-restrict-access/)
3. [Create a VPC](3-create-vpc-instance/)
4. [Set Up an EC2 Instance](4-create-ec2-instance/)
5. [Create a DocumentDB Intance](5-create-documentdb-instance/)
6. [Deploying the Application to EC2](6-deploy-the-application-to-ec2/)
7. [Create a CloudFront](7-create-cloudfront/)
8. [Cost calculation & Optimization](8-cost-calculation-optimization/)
9. [Clean Up Resources](9-clean-up/)