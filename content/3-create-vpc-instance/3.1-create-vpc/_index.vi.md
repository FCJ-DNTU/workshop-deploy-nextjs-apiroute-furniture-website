---
title: "Khởi tạo VPC Instance"
date: 2025
weight: 1
chapter: false
pre: "<b>3.1. </b>"
---

#### Khái niệm về VPC

Trong Workshop này, **Amazon Virtual Private Cloud (VPC)** sẽ được sử dụng để tạo một mạng riêng ảo (private network). Điều này giúp tổ chức và quản lý các tài nguyên một cách an toàn trong môi trường mạng tách biệt.

VPC cho phép:

- Kiểm soát địa chỉ IP, bảng định tuyến (Route Table), Internet Gateway và mạng con (Subnet).

- Cấu hình bảo mật và quyền truy cập giữa các dịch vụ trong AWS.

- Bảo vệ cơ sở dữ liệu Amazon DocumentDB bằng cách đặt trong Private Subnet, tránh truy cập từ Internet.

#### Mô hình triển khai VPC trong Workshop

Trong Workshop này, chúng ta sẽ khởi tạo VPC Instance bao gồm:

- 2 Availability Zones (AZs) để đảm bảo tính sẵn sàng cao.

- 2 Public Subnets (dành cho EC2 chạy ứng dụng).

- 2 Private Subnets (dành cho Amazon DocumentDB).

- Internet Gateway (IGW) để EC2 có thể giao tiếp với Internet.

- Security Group để quản lý quyền truy cập an toàn.
  ![use-case-vpc](/images/3-create-vpc-instance/3.1-create-vpc/use-case-vpc.png)

#### Chi phí cho VPC

Sẽ không phát sinh chi phí khi sử dụng VPC. Tuy nhiên, sẽ có phí dịch vụ cho những **dịch vụ của VPC**, ví dụ NAT Gateways, IP Address Manager, traffic mirroring, Reachability Analyzer, và Network Access Analyzer.

#### Khởi tạo VPC Instance

Ở section này chúng ta sẽ khởi tạo **VPC Instance**, bao gồm 2 **AZ**, 2 Public Subnets, 2 Private Subnets

#### 1. Tạo VPC Instance

- Truy cập [Your VPCs](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#vpcs:)
- Chọn **Create VPC**
  ![access-vpc](/images/3-create-vpc-instance/3.1-create-vpc/3.1.png)

- Tại **VPC Settings**, chọn option **VPC and more**

![create-vpc](/images/3-create-vpc-instance/3.1-create-vpc/3.2.png)

- Đặt tên **name tag** và để mặc định các field khác, chọn **Create VPC**
  ![create-vpc-done](/images/3-create-vpc-instance/3.1-create-vpc/3.3.png)
- Chọn **View VPC** để xem chi tiết VPC đã tạo
  ![review-result](/images/3-create-vpc-instance/3.1-create-vpc/3.4.png)

#### 2. Tiếp theo chúng ta sẽ gán Public IP4 cho các public subnet

- Truy cập [Subnets](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#subnets:)
- Chọn **Subnet ID** của public subnet e.g. **deploy-nextjs-workshop-subnet-public1-ap-southeast-1a**
  ![subnets](/images/3-create-vpc-instance/3.1-create-vpc/3.5.png)
- Chọn dropdown **Actions**, **Edit subnet settings**
  ![edit-subnet](/images/3-create-vpc-instance/3.1-create-vpc/3.6.png)
- Tích vào **Enable auto-assign public IPv4 address**, **Save**
  ![enable-ipv4](/images/3-create-vpc-instance/3.1-create-vpc/3.7.png)
- Hoàn thành gán Public IPv4 cho một public subnet **deploy-nextjs-workshop-subnet-public1-ap-southeast-1a**
  ![complete](/images/3-create-vpc-instance/3.1-create-vpc/3.8.png)
