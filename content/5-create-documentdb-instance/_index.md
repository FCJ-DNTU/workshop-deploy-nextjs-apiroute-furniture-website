---
title: "Create AWS DocumentDB Service"
date: 2025
weight: 5
chapter: false
pre: "<b>5. </b>"
---

#### AWS Relational Database Service

**Amazon Relational Database Service (Amazon RDS)** is a web service that makes it easier to set up, operate, and scale
a relational database in the AWS Cloud.

#### Create a DB Instance on AWS

{{% notice info %}}
Note: The procedure below assumes that **Standard create** is enabled and **Easy create** is not. This procedure uses
MySQL as an example.
{{% /notice %}}

#### Steps to create a DB Instance:

1. Log in to the **AWS Management Console** and open the Amazon RDS console at [RDS
Console](https://console.aws.amazon.com/rds/).

2. In the top-right corner of the **Amazon RDS** console, select the AWS region where you want to create the DB
Instance.

3. In the navigation pane, select **Databases**.

4. Click **Create database**, then choose **Standard create**.

![rds-interface.png](/images/5-create-rds-instance/rds-interface.png)

5. For **Engine type**, select MariaDB, Microsoft SQL Server, MySQL, Oracle, or PostgreSQL. In this example, we use
**MySQL**.

6. For **Edition**, select **MySQL Community**.

7. For **Version**, select the engine version (e.g., MySQL 8.0.39).
![create-db.png](/images/5-create-rds-instance/create-db.png)

8. Under **Templates**, choose the **Free tiers** template.

9. To set the master password, follow these steps:

- In the **Settings** section, open **Credential Settings**.
- If **Auto generate a password** is checked, uncheck it if you want to specify a password.
- (Optional) Change the **Master username** value.
- Enter the same password in **Master password** and **Confirm password**.
- (Optional) Set up connection with a compute resource for the DB Instance.

![settings.png](/images/5-create-rds-instance/settings.png)

10. You can configure the connection between an Amazon EC2 instance and the new DB Instance during the creation process.

- Under **Connectivity**, choose **Connect to EC2 Compute Resource**.
- Select the **EC2 Instance** you created earlier.

11. Configure **DB Subnet Group**:

- In the **DB Subnet Group** section, choose **Choose Existing** and select **golang-db-subnet-group**.

12. Configure **VPC Security Group**:

- Select **Choose Existing**, and from the dropdown, select **private-sg**, the security group you created earlier.

![connectivity.png](/images/5-create-rds-instance/connectivity.png)

13. Click **Create database**.

14. Verify the RDS Instance:

- In the RDS instance details page, you can find information like **Endpoint**, **Port**, and **Username**.
- The **Endpoint** is the URL or IP address you use to connect to the RDS database.

![rds.png](/images/5-create-rds-instance/rds.png)

15. Verify MySQL connection from EC2:

```shell
$ sudo yum install mysql
$ mysql -h <endpoint> -P 3306 -u admin -p <password>
      # $ mysql -h mysql-golang-db.c1a20mqwgeb9.ap-southeast-1.rds.amazonaws.com -P 3306 -u admin -pAdmin123
      ```

      ![ec2-to-mysql.png](/images/5-create-rds-instance/ec2-to-mysql.png)

      16. Create a Database:
      {{% notice tip %}}
      The admin user cannot directly access the **mysql** database. It is recommended to create a new database for
      access. This issue is due to RDS privileges.
      {{% /notice %}} > Since RDS is a managed service, to maintain system integrity and stability, superuser privileges
      are not granted to even the master user of the DB instance. Therefore, errors like this are expected, as the RDS
      MySQL master user by default does not have ADMIN, ROLE_ADMIN, or SUPER privileges.

      ```mysql
      CREATE DATABASE blog_db;
      # Query OK, 1 row affected (0.01 sec)
      ```

      17. Access the Database:
      ![db.png](/images/5-create-rds-instance/db.png)