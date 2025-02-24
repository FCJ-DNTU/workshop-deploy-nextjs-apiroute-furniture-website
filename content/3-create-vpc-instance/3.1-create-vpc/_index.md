---
title: "Create a VPC Instance"
date: 2025
weight: 1
chapter: false
pre: "<b>3.1. </b>"
---

#### Introduction to VPC

In this workshop, _Amazon Virtual Private Cloud (VPC)_ is used to create a **private network**, enabling the organization and management of resources within a dedicated network space on AWS.

VPC allows you to have complete control over your network environment, including IP address configuration, route tables, internet gateways, and subnets.

#### Key Use Cases of VPC in the Workshop

- **Hosting and running applications on EC2:** Provides a secure environment for deploying virtual servers (instances).

- **Interacting with RDS databases:** Creates a private subnet to protect the database from internet access, allowing connections only from services within the VPC.

- **Using S3 via Gateway:** Ensures secure data transmission between S3 and services in the VPC through an S3 Gateway without exposing it to the public internet.

![use-case-vpc](/images/3-create-vpc-instance/3.1-create-vpc/use-case-vpc.png)

#### Pricing for VPC

There's no additional charge for using a VPC. However, charges for some **VPC components**, such as NAT gateways, IP Address Manager, traffic mirroring, Reachability Analyzer, and Network Access Analyzer.

#### Create a VPC Instance

In this section, we will create a **VPC Instance**, which includes 2 **Availability Zones (AZs)**, **2 Public Subnets** and **2 Private Subnets.**

#### 1. Create the VPC Instance

- Go to [Your VPCs](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#vpcs:), and select **Create VPC**.
- In the **VPC Settings**, choose the option **VPC and more**.

![create-vpc](/images/3-create-vpc-instance/3.1-create-vpc/create-vpc.png)

- Add a **name tag** and keep the default values for other fields, then click **Create VPC**.
  ![create-vpc-done](/images/3-create-vpc-instance/3.1-create-vpc/create-vpc-done.png)
- The VPC Instance is successfully created.
  ![review-result](/images/3-create-vpc-instance/3.1-create-vpc/review-result.png)

#### 2. Assign Public IPv4 to Public Subnets

- Go to [Subnets](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#subnets:).
- Select the **Subnet ID** of the public subnet, e.g., **deploy-golang-workshop-subnet-public1-ap-southeast-1a**.
  ![subnets](/images/3-create-vpc-instance/3.1-create-vpc/subnets.png)
- From the dropdown **Actions**, choose **Edit subnet settings**.
  ![edit-subnet](/images/3-create-vpc-instance/3.1-create-vpc/edit-subnet.png)
- Check the option **Enable auto-assign public IPv4 address**, and click Save.
  ![enable-ipv4](/images/3-create-vpc-instance/3.1-create-vpc/enable-ipv4.png)
- Successfully assigned a Public IPv4 address to the public subnet **deploy-golang-workshop-subnet-public1-ap-southeast-1a**.
  ![complete](/images/3-create-vpc-instance/3.1-create-vpc/complete.png)
