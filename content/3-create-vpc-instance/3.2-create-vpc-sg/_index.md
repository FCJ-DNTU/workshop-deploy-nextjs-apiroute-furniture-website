---
title: "Create Security Group"
date: 2025
weight: 2
chapter: false
pre: "<b>3.2. </b>"
---

#### VPC Security Group

A Security Group is a set of rules that control inbound and outbound network traffic for instances in an Amazon VPC. It
acts as a virtual firewall, managing and protecting resources.

#### Key Features of Security Group

- **Allow Rules Only**: Does not support deny rules.
- **Traffic Segmentation**: Separate rules for inbound and outbound traffic.
- **No Default Inbound Rules**: Must add inbound rules to allow access.
- **Default Outbound Rule**: Allows all outbound traffic, which can be modified.
- **Stateful**: If inbound traffic is allowed, outbound responses are automatically permitted.
- **Instance Connectivity**: Only connects if allowed by rules.
- **Linked to Network Interface**: Can change the security group after creation.

#### Create VPC Security Groups

Next, we will create two Security Groups (SGs) representing **public subnets (EC2)** and **private subnets
(DocumentDB)**.

#### 2.1. Create Public Security Group (SG Public - EC2)

- Go to [Security
groups](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#SecurityGroups:) and select
**Create security group**
![security-groups](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.1.png)
- In the **Create Security Group** interface, provide the following details for **public-sg-ec2**:
- **Security group name**: `public-sg-ec2`
- **Description**: `Public Security Group for EC2`
- **VPC**: Select your existing VPC
![basic-details](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.2.png)
- Add the following **Inbound Rules** for **public-sg-ec2**
| Protocol | Port Range | Source | Description |
| -------- | ---------- | ------ | ------------ |
| HTTP | 80 | 0.0.0.0/0 | Allow website access |
| HTTPS | 443 | 0.0.0.0/0 | Enable SSL/TLS |
| SSH | 22 | 0.0.0.0/0 | SSH access to EC2 |
| Custom TCP | 3000 | 0.0.0.0/0 | Run Next.js in dev mode |
| Custom TCP | 27017 | SG-Private-DocumentDB | Allow EC2 to connect to DocumentDB |

![inbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.3.png)
**❗❗ Add Inbound rule for port 27017 later**

- Allow all **Outbound** traffic and select **Create security group**
![outbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.4.png)
![public-sg](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.5.png)

#### 2.2. Create Private Security Group (SG Private - DocumentDB)

- In the **Create Security Group** interface, provide the following details for **private-sg-documentdb**:
- **Security group name**: `private-sg-documentdb`
- **Description**: `Private Security Group for DocumentDB`
- **VPC**: Select your existing VPC
![basic-details](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.6.png)
- Add the following **Inbound Rules** for **private-sg-documentdb**

| Protocol | Port Range | Source | Description |
| ---------- | ---------- | ------------- | ------------------------------ |
| Custom TCP | 27017 | SG-Public-EC2 | Only EC2 can access DocumentDB |

![inbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.7.png)

- Allow all **Outbound** traffic and select **Create security group**
![outbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.4.png)
![public-sg](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.8.png)

#### 2.3 Add **Inbound Rules** for Port 27017 in **public-sg-ec2**

- In the **Security Groups** interface, select **public-sg-ec2**
![public-sg-ec2](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.9.png)

- Then, select **Edit inbound rules** to add an inbound rule
![add-inbound](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.10.png)

- Add the inbound rule and select **Save rules**
![inbound](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.11.png)
![inbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.12.png)