---
title: "Clean Up Resources"
date: 2025
weight: 9
chapter: false
pre: "<b>9. </b>"
---

#### 1. Delete CloudFront Distribution

- Go to [CloudFront](https://us-east-1.console.aws.amazon.com/cloudfront/v4/home#/distributions)
- Select the **distribution** to delete
- Click **Disable**
- Once **Disabled successfully**, click **Delete** to remove it
  ![delete-distribution.png](/images/9-clean-up/9.1.png)
  ![delete-distribution.png](/images/9-clean-up/9.2.png)

#### 2. Delete DocumentDB Cluster

- Go to Amazon DocumentDB, section
  [Clusters](https://ap-southeast-1.console.aws.amazon.com/docdb/home?region=ap-southeast-1#clusters)
- Select the **Cluster to delete**
- Click the **Action** dropdown, then select **Delete**
- A confirmation dialog will appear
- Name the **Snapshot**: `nextjs-cluster` and type `delete entire cluster`
  ![delete-cluster.png](/images/9-clean-up/9.3.png)
  ![delete-cluster.png](/images/9-clean-up/9.4.png)
- After **successfully deleting the Cluster**, go to
  [Snapshots](https://ap-southeast-1.console.aws.amazon.com/docdb/home?region=ap-southeast-1#snapshots) to delete the
  snapshot
- Select the **Snapshot to delete**
- Click the **Action** dropdown, then select **Delete**
  ![delete-snapshot.png](/images/9-clean-up/9.5.png)

#### 3. Delete EC2 Resources

- Go to [EC2 Instances](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Instances:)
- Select the **Instance**, click the **Instance state** dropdown, and choose **Terminate**.
  ![delete-ec2.png](/images/9-clean-up/9.6.png)

#### 4. Delete VPC Resources

- Go to [Your VPCs](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#vpcs:)
- Select the **Instance**, then click **Actions** and choose **Delete VPC**.
  ![delete-vpc.png](/images/9-clean-up/9.7.png)

#### 5. Clean up IAM Resources

- Access **Root user**.
- Go to [IAM Service](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Under **Policies**, select the **Policy** and choose **Delete**

  ![delete-policy.png](/images/9-clean-up/9.8.png)

- Under **User groups**, select the **Groups** and click **Delete**
  ![delete-groups.png](/images/9-clean-up/9.9.png)

- Under **Users**, select the user and click **Delete**.

  ![delete-users.png](/images/9-clean-up/9.10.png)

{{< center >}}

### **Done! 🚀**

{{< /center >}}
