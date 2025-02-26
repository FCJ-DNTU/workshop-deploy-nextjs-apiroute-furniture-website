---
title: "Create a VPC Instance"
date: 2025
weight: 1
chapter: false
pre: "<b>3.1. </b>"
---

#### Introduction to VPC

In this workshop, **Amazon Virtual Private Cloud (VPC)** will be used to create a private network. This helps organize
and manage resources securely in an isolated network environment.

VPC allows:

- Controlling IP addresses, route tables, Internet Gateway, and subnets.

- Configuring security and access control between AWS services.

- Protecting Amazon DocumentDB by placing it in a Private Subnet, preventing access from the Internet.

#### VPC Deployment Model in This Workshop

In this workshop, we will initialize a VPC Instance that includes:

- 2 Availability Zones (AZs) to ensure high availability.

- 2 Public Subnets (for EC2 running applications).

- 2 Private Subnets (for Amazon DocumentDB).

- An Internet Gateway (IGW) to allow EC2 instances to communicate with the Internet.

- Security Groups to manage secure access control.

  ![use-case-vpc](/images/3-create-vpc-instance/3.1-create-vpc/use-case-vpc.png)

#### Pricing for VPC

There is no cost associated with using VPC itself. However, charges apply for **VPC-related services**, such as NAT Gateways, IP Address Manager, traffic mirroring, Reachability Analyzer, and Network Access Analyzer.

#### Create a VPC Instance

In this section, we will create a **VPC Instance**, which includes 2 **Availability Zones (AZs)**, **2 Public Subnets** and **2 Private Subnets.**

#### 1. Create the VPC Instance

- Go to [Your VPCs](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#vpcs:), and
- Select **Create VPC**.
  ![access-vpc](/images/3-create-vpc-instance/3.1-create-vpc/3.1.png)
- In the **VPC Settings**, choose the option **VPC and more**.

  ![create-vpc](/images/3-create-vpc-instance/3.1-create-vpc/3.2.png)

- Enter a **name tag** and leave other fields as default, then click **Create VPC**.
  ![create-vpc-done](/images/3-create-vpc-instance/3.1-create-vpc/3.3.png)
- Click View VPC to see the details of the created VPC
  ![review-result](/images/3-create-vpc-instance/3.1-create-vpc/3.4.png)

#### 2. Assign Public IPv4 to Public Subnets

- Go to [Subnets](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#subnets:).
- Select the **Subnet ID** of the public subnet, e.g., **deploy-nextjs-workshop-subnet-public1-ap-southeast-1a**.
  ![subnets](/images/3-create-vpc-instance/3.1-create-vpc/3.5.png)
- Click the **Actions** dropdown, then select **Edit subnet settings**
  ![edit-subnet](/images/3-create-vpc-instance/3.1-create-vpc/3.6.png)
- Check **Enable auto-assign public IPv4 address**, then **Save**
  ![enable-ipv4](/images/3-create-vpc-instance/3.1-create-vpc/3.7.png)
- Successfully assigned a Public IPv4 to the public subnet **deploy-nextjs-workshop-subnet-public1-ap-southeast-1a**
  ![complete](/images/3-create-vpc-instance/3.1-create-vpc/3.8.png)
