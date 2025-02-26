---
title: "Khởi tạo Security Group"
date: 2025
weight: 2
chapter: false
pre: "<b>3.2. </b>"
---

#### Khái niệm về VPC Security Group

Security Group là một tập hợp các quy tắc kiểm soát lưu lượng mạng vào (Inbound) và ra (Outbound) của các instance trong Amazon VPC. Nó hoạt động như một tường lửa ảo, giúp quản lý và bảo vệ tài nguyên.

#### Đặc Điểm Chính Của Security Group

- **Chỉ cho phép (Allow Rules Only):** Không hỗ trợ quy tắc từ chối.
- **Tách biệt lưu lượng:** Quy tắc riêng cho vào (Inbound) và ra (Outbound).
- **Không có quy tắc vào mặc định:** Cần thêm inbound rule để cho phép truy cập.
- **Quy tắc ra mặc định:** Cho phép mọi lưu lượng ra, có thể chỉnh sửa.
- **Trạng thái (Stateful):** Nếu cho phép vào, ra cũng được tự động cho phép.
- **Kết nối giữa các instance:** Chỉ kết nối nếu có quy tắc cho phép.
- **Liên kết với giao diện mạng:** Có thể thay đổi security group sau khi tạo.

#### Khởi tạo VPC Security Group

Tiếp theo, chúng ta sẽ khởi tạo 2 SGs đại diện cho public subnets (EC2) và private subnets (DocumentDB)

#### 2.1. Khởi tạo Public Security Group (SG Public - EC2)

- Truy cập [Security groups](https://ap-southeast-1.console.aws.amazon.com/vpcconsole/home?region=ap-southeast-1#SecurityGroups:) và chọn **Create security group**
  ![security-groups](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.1.png)
- Trong Interface **Create Security Group**, thêm những thông tin sau cho **public-sg-ec2**

  - **Security group name:** `public-sg-ec2`
  - **Description:** `Public Security Group for EC2`
  - **VPC:** Chọn VPC bạn đang sử dụng
    ![basic-details](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.2.png)

- Thêm **Inbound Rules** tương ứng cho **public-sg-ec2**

  | Protocol   | Port Range | Source                | Mô tả                           |
  | ---------- | ---------- | --------------------- | ------------------------------- |
  | HTTP       | 80         | 0.0.0.0/0             | Cho phép truy cập website       |
  | HTTPS      | 443        | 0.0.0.0/0             | Hỗ trợ SSL/TLS                  |
  | SSH        | 22         | My IP Address         | Đăng nhập SSH vào EC2           |
  | Custom TCP | 3000       | 0.0.0.0/0             | Chạy Next.js dev mode           |
  | Custom TCP | 27017      | SG-Private-DocumentDB | Cho phép EC2 kết nối DocumentDB |

  ![inbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.3.png)
  **❗❗Bổ sung Inbound cho port 27017 sau**

- Allows all **Outbound** traffic và chọn **Create security group**
  ![outbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.4.png)
  ![public-sg](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.5.png)

#### 2.2 Khởi tạo Private Security Group (SG Private - DocumentDB)

- Trong Interface **Create Security Group**, thêm những thông tin sau cho **private-sg-documentdb**

  - **Security group name:** `private-sg-documentdb`
  - **Description:** `Private Security Group for DocumentDB`
  - **VPC:** Chọn VPC bạn đang sử dụng
    ![basic-details](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.6.png)

- Thêm **Inbound Rules** tương ứng cho **private-sg-documentdb**

  | Protocol   | Port Range | Source        | Mô tả                              |
  | ---------- | ---------- | ------------- | ---------------------------------- |
  | Custom TCP | 27017      | SG-Public-EC2 | Chỉ EC2 có thể truy cập DocumentDB |

  ![inbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.7.png)

- Allows all **Outbound** traffic và chọn **Create security group**
  ![outbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.4.png)
  ![public-sg](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.8.png)

#### 2.3 Thêm **Inbound Rules** cho Port 27017 trong **public-sg-ec2**

- Trong Interface **Security Groups**, chọn **public-sg-ec2**
  ![public-sg-ec2](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.9.png)

- Sau đó, chọn **Edit inbound rules** để thêm inbound rule
  ![add-inbound](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.10.png)

- Thêm inbound và chọn **Save rules**
  ![inbound](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.11.png)
  ![inbound-rules](/images/3-create-vpc-instance/3.2-create-vpc-sg/3.2.12.png)
