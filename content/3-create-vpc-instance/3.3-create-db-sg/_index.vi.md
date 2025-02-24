---
title: "Tạo Database Subnet Group"
date: 2025
weight: 3
chapter: false
pre: "<b>3.3 </b>"
---

#### Tạo DB Subnet Group trên AWS

Để tạo DB Subnet Group trên AWS, làm theo các bước sau:

1. Truy cập vào AWS Management Console.

2. Tìm và chọn dịch vụ **Amazon RDS**.

   ![rds-interface.png](/images/3-create-vpc-instance/3.3-create-db-sg/rds-interface.png)

3. Trong menu điều hướng, chọn [Subnet groups](https://ap-southeast-1.console.aws.amazon.com/rds/home?region=ap-southeast-1#db-subnet-groups-list:).

4. Chọn Create DB Subnet Group.

5. Trong giao diện Create DB Subnet Group:

   - Đặt tên cho subnet group của bạn trong trường Name.

   - Nhập mô tả cho subnet group của bạn trong trường Description.

   - Chọn Virtual Private Cloud (VPC) mặc định hoặc VPC bạn đã tạo.

6. Trong phần Add subnets, chọn các Availability Zones (AZ) chứa các subnets trong mục Availability Zones, sau đó chọn các subnets trong mục Subnets.

{{% notice note %}}
Nếu bạn đã bật Local Zone, bạn có thể chọn một nhóm Availability Zone trên trang Create DB Subnet Group. Trong trường hợp này, hãy chọn nhóm Availability Zone, các Availability Zones và Subnets tương ứng.
Sau khi hoàn thành, DB Subnet Group mới của bạn sẽ xuất hiện trong danh sách các DB Subnet Group trên giao diện RDS console. Bạn có thể chọn DB Subnet Group để xem chi tiết, bao gồm danh sách các subnets được kết nối với nhóm này, trong phần chi tiết ở dưới cùng của cửa sổ.
{{% /notice %}}

7. Nhấn nút Create để hoàn thành quá trình tạo DB Subnet Group.

![complete.png](/images/3-create-vpc-instance/3.3-create-db-sg/complete.png)
