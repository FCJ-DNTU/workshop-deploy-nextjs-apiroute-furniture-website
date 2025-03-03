---
title: "Dọn dẹp tài nguyên"
date: 2025
weight: 9
chapter: false
pre: "<b>9. </b>"
---

#### 1. Xóa CloudFront Distribution

- Truy cập [CloudFront](https://us-east-1.console.aws.amazon.com/cloudfront/v4/home#/distributions)
- Chọn **distribution** cần xóa
- Chọn **Disable**
- Sau khi **Disable thành công** thì chọn **Delete** để xóa
  ![delete-distribution.png](/images/9-clean-up/9.1.png)
  ![delete-distribution.png](/images/9-clean-up/9.2.png)

#### 2. Xóa Cluster DocumentDB

- Truy cập Amazon DocumentDB, ở phần
  [Clusters](https://ap-southeast-1.console.aws.amazon.com/docdb/home?region=ap-southeast-1#clusters)
- Chọn **Cluster cần xóa**
- Chọn dropdown **Action**, **Delete**
- Khi chọn **Delete** sẽ xuất hiện một bảng thông báo
- Đặt tên cho **Snapshot**: `nextjs-cluster` và gõ `delete entire cluster`
  ![delete-cluster.png](/images/9-clean-up/9.3.png)
  ![delete-cluster.png](/images/9-clean-up/9.4.png)

- Sau khi xóa **Cluster thành công**, vào phần [Snapshots](https://ap-southeast-1.console.aws.amazon.com/docdb/home?region=ap-southeast-1#snapshots) để xóa snapshot
- Chọn **Snapshot cần xóa**
- Chọn dropdown **Action**, **Delete**
  ![delete-snapshot.png](/images/9-clean-up/9.5.png)

#### 3. Xóa tài nguyên EC2

- Truy cập [EC2 Instance](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Instances:)
- Chọn **Instance**, dropdown **Instance state**, chọn **Terminate**
  ![delete-ec2.png](/images/9-clean-up/9.6.png)

#### 4. Xóa tài nguyên VPC

- Truy cập [Your VPCs](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#vpcs:)
- Chọn **Instance** > **Actions** > **Delete VPC**
  ![delete-vpc.png](/images/9-clean-up/9.7.png)

#### 5. Xóa tài nguyên trong IAM Service

- Truy cập vào **Root user**
- Truy cập [IAM Service](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Chọn **Policies**, thực hiện chọn **Policy** và **Delete**

![delete-policy.png](/images/9-clean-up/9.8.png)

- Trong **User groups**, chọn **Groups**, thực hiện **Delete**

![delete-groups.png](/images/9-clean-up/9.9.png)

- Chọn **Users**, thực hiện **Delete**

![delete-users.png](/images/9-clean-up/9.10.png)

{{< center >}}

### **Hoàn thành! 🚀**

{{< /center >}}
