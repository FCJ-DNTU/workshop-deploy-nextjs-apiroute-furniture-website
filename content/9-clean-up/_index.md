---
title: "Clean Up Resources"
date: 2025
weight: 9
chapter: false
pre: "<b>9. </b>"
---

![Workshop Architecture](/images/workshop_architecture.png)

#### 1. Delete S3 Resources

- Go to [Amazon S3](https://ap-southeast-1.console.aws.amazon.com/s3/buckets?region=ap-southeast-1&bucketType=general)
- Select the **bucket instance**, click **empty** to delete all objects in the bucket.
- Then perform **Delete**.
![delete-s3.png](/images/8-clean-up/delete-s3.png)

#### 2. Delete RDS Resources

- Go to Amazon RDS, section
[Database](https://ap-southeast-1.console.aws.amazon.com/rds/home?region=ap-southeast-1#databases:)
- Click the **Action** dropdown and select **Delete**.
![delete-rds.png](/images/8-clean-up/delete-rds.png)

#### 3. Delete EC2 Resources

- Go to [EC2 Instances](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Instances:)
- Select the **instance**, click the **Instance state** dropdown, and choose **Terminate**.
![delete-ec2.png](/images/8-clean-up/delete-ec2.png)

#### 4. Delete VPC Resources

- Go to [Your VPCs](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#vpcs:)
- Select the **Instance**, then click **Actions** and choose **Delete VPC**.
![delete-vpc.png](/images/8-clean-up/delete-vpc.png)

#### 5. Clean up IAM Resources

- Access **Root user**.
- Go to [IAM Service](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Under **Policies**, select the **Policy** and choose **Delete**.

![delete-policy.png](/images/8-clean-up/delete-policy.png)

- Under **Groups**, select the group and click **Delete**.

![delete-groups.png](/images/8-clean-up/delete-groups.png)

- Under **Users**, select the user and click **Delete**.

![delete-users.png](/images/8-clean-up/delete-users.png)

# COOKED ⭐