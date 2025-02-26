---
title: "Tạo Database Subnet Group"
date: 2025
weight: 3
chapter: false
pre: "<b>3.3 </b>"
---

#### DB Subnet Group là gì?

**DB Subnet Group** trong Amazon DocumentDB là một tập hợp các subnets trong VPC, được sử dụng để xác định các subnets mà cluster DocumentDB có thể triển khai. Mỗi DB Subnet Group phải chứa **tối thiểu hai subnets thuộc các Availability Zones (AZs) khác nhau** để đảm bảo tính sẵn sàng cao và khả năng dự phòng.

#### Tại sao cần tạo DB Subnet Group?

- **Hỗ trợ triển khai trong VPC**: DB Subnet Group cho phép chỉ định **VPC và subnets cụ thể** để triển khai Amazon DocumentDB cluster, giúp kiểm soát phạm vi mạng và bảo mật tốt hơn.

- **Đảm bảo tính sẵn sàng & dự phòng**: Amazon DocumentDB yêu cầu **ít nhất hai subnets thuộc các Availability Zones (AZs) khác nhau** để đảm bảo khả năng chịu lỗi. Nếu một AZ gặp sự cố, cluster vẫn có thể hoạt động bình thường trên AZ còn lại.

- **Tự động chuyển đổi khi lỗi (Failover)**: Nếu **primary instance** gặp lỗi, DocumentDB có thể tự động cấp một **replica instance** trong một subnet khác làm primary mới. Sau đó, một replica mới sẽ được tạo trong subnet trước đó để duy trì số lượng node cần thiết.

#### Tạo DB Subnet Group trên AWS

Để tạo DB Subnet Group trên AWS, làm theo các bước sau:

1. Truy cập vào **AWS Management Console**.

2. Tìm và chọn dịch vụ **Amazon DocumentDB**.

3. Trong menu điều hướng, chọn [Subnet
   groups](hhttps://ap-southeast-1.console.aws.amazon.com/docdb/home?region=ap-southeast-1#subnetGroups).
   ![documentdb-interface.png](/images/3-create-vpc-instance/3.3-create-db-sg/3.3.1.png)

4. Trong **Subnet groups**, chọn **Create**

5. Trong giao diện **Create DB Subnet Group**:

   - **Name**: `nextjs-db-subnet-group`.

   - **Description**: `nextjs-db-subnet-group`.

   - Chọn VPC mà bạn đang sử dụng.

   - Trong phần **Add Subnets**, chọn các **Private Subnet**:
     - Chọn **ít nhất hai Private Subnet thuộc hai AZs khác nhau** (ví dụ subnet-xxxxxx1 ở AZ **us-east-1a**, subnet-xxxxxx2 ở AZ **us-east-1b**).
     - Nhấn **Add subnet**
       ![create-subnet-group.png](/images/3-create-vpc-instance/3.3-create-db-sg/3.3.2.png)

6. Nhấn nút **Create** để hoàn thành quá trình tạo DB Subnet Group.

![complete.png](/images/3-create-vpc-instance/3.3-create-db-sg/3.3.3.png)

{{< center >}}

### **Hoàn thành! 🚀**

{{< /center >}}
