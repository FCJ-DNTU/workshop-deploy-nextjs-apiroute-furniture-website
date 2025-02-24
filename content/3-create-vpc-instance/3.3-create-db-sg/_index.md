---
title: "Create Database Subnet Group"
date: 2025
weight: 3
chapter: false
pre: "<b>3.3 </b>"
---

#### Create a DB Subnet Group on AWS

Follow these steps to create a **DB Subnet Group** on AWS:

1. Go to the **AWS Management Console**.

2. Search for and select the **Amazon RDS** service.

   ![rds-interface.png](/images/3-create-vpc-instance/3.3-create-db-sg/rds-interface.png)

3. In the navigation menu, select [Subnet groups](https://ap-southeast-1.console.aws.amazon.com/rds/home?region=ap-southeast-1#db-subnet-groups-list:).

4. Click **Create DB Subnet Group**.

5. In the **Create DB Subnet Group** page:

   - Enter a **Name** for your subnet group.
   - Provide a **Description** for the subnet group.
   - Select the **VPC** (use the one you created or the default VPC).

6. In the **Add subnets** section:
   - Select the **Availability Zones (AZs)** where your subnets are located.
   - Choose the corresponding **Subnets** for each AZ.

> **Note:**  
> If you have enabled Local Zones, you can select an Availability Zone Group on the **Create DB Subnet Group** page. In this case, pick the AZ Group, the individual AZs, and the corresponding subnets.  
> Once complete, your new DB Subnet Group will appear in the list of DB Subnet Groups on the RDS console. You can view its details, including a list of subnets associated with the group, in the details section at the bottom of the page.

7. Click **Create** to finalize the creation of the DB Subnet Group.

![complete.png](/images/3-create-vpc-instance/3.3-create-db-sg/complete.png)
