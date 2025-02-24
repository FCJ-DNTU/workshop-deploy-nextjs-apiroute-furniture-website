---
title: "Khởi tạo Security Group"
date: 2025
weight: 2
chapter: false
pre: "<b>3.2. </b>"
---

#### Khái niệm về VPC Security Group

Security Group là một tập hợp các subnet chạy trong môi trường Amazon Virtual Private Cloud (VPC), cho phép bạn chỉ định các quy tắc kiểm soát lưu lượng đến và đi.

#### Khởi tạo VPC Security Group

Tiếp theo, chúng ta sẽ khởi tạo 2 SGs đại diện cho public subnets (EC2 instance) và private subnets (RDS instance)

Truy cập vào **VPC Service**. Tại thanh điều huướng bên trái, chọn [Security Group](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#SecurityGroups:). Chọn **create security group**
Tạo 2 Security Group

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

#### 2.1. Khởi tạo Public SG

- Trong interface **Create Security Group**, thêm **Basic Details** cho **private-sg**
- Thêm **Inbound Rules** tương ứng cho **public-sg**
- Allows all **outbound** traffic
- Chọn **Create security group**
  ![public-sg](/images/3-create-vpc-instance/3.2-create-vpc-sg/public-sg.png)

#### 2.2 Khởi tạo Private SG

- Trong interface **Create Security Group**, thêm **Basic Details** cho **private-sg**
- Thêm **Inbound Rules** tuong ứng cho **private-sg**
- Allows all **outbound** traffic
- Chọn **Create security group**
  ![private-sg.png](/images/3-create-vpc-instance/3.2-create-vpc-sg/private-sg.png)
