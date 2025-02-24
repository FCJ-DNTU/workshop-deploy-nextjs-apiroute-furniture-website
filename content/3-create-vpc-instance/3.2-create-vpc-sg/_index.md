---
title: "Create Security Group"
date: 2025
weight: 2
chapter: false
pre: "<b>3.2. </b>"
---

#### VPC Security Group

A subnet group is a collection of subnets running on Amazon Virtual Private Cloud (VPC) environment, allows you configure **inbound and outbound** rules

#### Create VPC Security Groups

In this step, we will create two **Security Groups (SGs)**: one for public subnets (used by EC2 instances) and one for private subnets (used by RDS instances).

- Go to the **VPC Service**. In the left-hand menu, select [Security Groups](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#SecurityGroups:). Click **Create Security Group**.
- Create the following two Security Groups:

| Subnet Name | Direction | Protocol    | Port Range | Source/Destination |
| ----------- | --------- | ----------- | ---------- | ------------------ |
| public-sg   | Inbound   | SSH (TCP)   | 22         | My IP Address      |
| public-sg   | Inbound   | ICMP        | All        | 0.0.0.0/0          |
| public-sg   | Inbound   | TCP         | All        | 0.0.0.0/0          |
| public-sg   | Inbound   | HTTP        | All        | 0.0.0.0/0          |
| public-sg   | Inbound   | HTTPS       | All        | 0.0.0.0/0          |
| public-sg   | Outbound  | All         | All        | 0.0.0.0/0          |
| private-sg  | Inbound   | MySQL (TCP) | 3306       | 0.0.0.0/0          |
| private-sg  | Outbound  | All         | All        | 0.0.0.0/0          |

---

#### 2.1. Create Public SG

- In the **Create Security Group** interface, fill out the **Basic Details** for **public-sg**.
- Add the **Inbound Rules** for **public-sg** as shown above.
- Allow all **Outbound** traffic.
- Click **Create Security Group**.
  ![public-sg](/images/3-create-vpc-instance/3.2-create-vpc-sg/public-sg.png)

---

#### 2.2. Create Private SG

- In the **Create Security Group** interface, fill out the **Basic Details** for **private-sg**.
- Add the **Inbound Rules** for **private-sg** as shown above.
- Allow all **Outbound** traffic.
- Click **Create Security Group**.
  ![private-sg.png](/images/3-create-vpc-instance/3.2-create-vpc-sg/private-sg.png)
