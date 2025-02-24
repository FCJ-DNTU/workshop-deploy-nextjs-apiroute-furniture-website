---
title: "Dọn dẹp tài nguyên"
date: 2025
weight: 8
chapter: false
pre: "<b>8. </b>"
---

![Workshop Architecture](/images/workshop_architecture.png)

#### 1. Xóa tài nguyên S3

- Truy cập [Amazon S3](https://ap-southeast-1.console.aws.amazon.com/s3/buckets?region=ap-southeast-1&bucketType=general)
- Chọn **bucket instance**, **empty** xóa hết đối tượng trong bucket
- Sau đó thực hiện **Delete**
  ![delete-s3.png](/images/8-clean-up/delete-s3.png)

#### 2. Xóa tài nguyên RDS

- Truy cập Amazon RDS, section [Database](https://ap-southeast-1.console.aws.amazon.com/rds/home?region=ap-southeast-1#databases:)
- Chọn dropdown **Action**, **Delete**
  ![delete-rds.png](/images/8-clean-up/delete-rds.png)

#### 3. Xóa tài nguyên EC2

- Truy cập [EC2 Instance](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Instances:)
- Chọn **instance**, dropdown **Instance state**, chọn **Terminate**
  ![delete-ec2.png](/images/8-clean-up/delete-ec2.png)

#### 4. Xóa tài nguyên VPC

- Truy cập [Your VPCs](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#vpcs:)
- Chọn **Instance** > **Actions** > **Delete VPC**
  ![delete-vpc.png](/images/8-clean-up/delete-vpc.png)

#### 4. Xóa tài nguyên trong IAM Service

- Truy cập vào **Root user**
- Truy cập [IAM Service](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Chọn **Policies**, thực hiện chọn **Policy** và **Delete**

![delete-policy.png](/images/8-clean-up/delete-policy.png)

- Chọn **Groups**, thực hiện **Delete**

  ![delete-groups.png](/images/8-clean-up/delete-groups.png)

- Chọn **Users**, thực hiện **Delete**

![delete-users.png](/images/8-clean-up/delete-users.png)

# COOKED ⭐
