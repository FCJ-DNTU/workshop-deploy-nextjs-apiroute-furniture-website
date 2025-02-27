---
title: "Create AWS DocumentDB"
date: 2025
weight: 5
chapter: false
pre: "<b>5. </b>"
---

![aws-documentdb.png](/images/5-create-documentdb-instance/aws-documentdb.png)

#### AWS DocumentDB

**Amazon DocumentDB** (with MongoDB compatibility) is a **fully managed native JSON document database** that simplifies running critical document workloads at virtually any scale without the need to manage infrastructure, making it cost-effective and easy to use.

#### Benefits of Using DocumentDB:

- **MongoDB Compatibility:** Supports popular MongoDB APIs, allowing easy migration from MongoDB.

- **High Performance & Scalable:** Distributed storage architecture enhances read/write speed and automatically scales as needed.

- **Easy Management:** As a **fully managed** service, it reduces database administration overhead.

#### 💰 Pricing of Amazon DocumentDB

The cost of **Amazon DocumentDB** depends on the following factors:

- **On-Demand Instances** – Charged per hour based on instance type.

- **Database I/O** – Charged per million read/write operations.

- **Database Storage** – Charged per GB per month.

- **Backup Storage** – Charged per GB per month beyond database storage.

AWS provides **two pricing options**:

- **Amazon DocumentDB Standard (Pay-as-you-go I/O)**

  - Suitable for workloads with low to medium I/O.
  - Charges include four factors: Instance, Database I/O, Storage, and Backup.
  - If I/O costs are less than 25% of the total cost, this is a suitable option.

- **Amazon DocumentDB I/O-Optimized (Includes I/O costs in instance pricing)**

  - Ideal for high I/O applications requiring predictable costs.
  - Charges only three factors: Instance, Storage, and Backup.
  - No separate I/O charges, making cost estimation easier.
  - If I/O costs exceed 25% of the total, this is the recommended choice.

#### Which Configuration to Choose for This Workshop?

- We will choose **Amazon DocumentDB Standard**
  to optimize costs because:

  - Initial query volume is low.
  - Pay only for actual I/O usage.
  - Flexible upgrade to I/O-Optimized when queries increase.

#### Creating a DB Instance on AWS

1. Log in to the **AWS Management Console** and open **Amazon DocumentDB**

2. Create a **DocumentDB Cluster**
   ![Cluster-interface.png](/images/5-create-documentdb-instance/5.1.png)

3. In **Create Amazon DocumentDB cluster**, enter the following details:

   - **Cluster type**: Instance-based cluster
   - **Cluster identifier**: `docdb-nextjs-workshop`
   - **Engine version**: Select the latest version
   - **Cluster storage configuration**: Amazon DocumentDB Standard
   - **Instance class**: db.t3.medium
   - **Number of instances**: 2 (1 Primary + 1 Replica)
   - **Connectivity**: Connect to an EC2 compute resource
   - **EC2 Instance**: Select the created EC2 instance
   - **Username**: `user123`
   - **Password**: `user1234`
   - **Subnet group**: Select the subnet group created in **3.3**
   - **VPC security groups**: **private-sg-documentdb (VPC)**
   - **Deletion protection**: Uncheck **Enable deletion protection**
   - Review the settings and click **Create cluster**
     ![create-1.png](/images/5-create-documentdb-instance/5.2.png)
     ![create-2.png](/images/5-create-documentdb-instance/5.3.png)
     ![create-3.png](/images/5-create-documentdb-instance/5.4.png)
     ![create-4.png](/images/5-create-documentdb-instance/5.5.png)
     ![create-5.png](/images/5-create-documentdb-instance/5.6.png)
     ![create-6.png](/images/5-create-documentdb-instance/5.7.png)
   - Cluster successfully created
     ![create-success.png](/images/5-create-documentdb-instance/5.8.png)

4. Verify Connection from EC2 to DocumentDB

- Open the newly created cluster, go to the **Configuration** tab, and copy the **Cluster endpoint**
  ![copy-cluster.png](/images/5-create-documentdb-instance/5.9.png)
- In EC2, click **Connect** and then click **Connect** under the **EC2 Instance Connect** tab.

  ```shell
  $ sudo apt-get install -y netcat
  $ nc -zv docdb-nextjs-workshop.cluster-c10k88ou8amc.ap-southeast-1.docdb.amazonaws.com 27017
  ```

  ![success.png](/images/5-create-documentdb-instance/5.10.png)

{{< center>}}

### **Completed! 🚀**

{{< /center>}}
