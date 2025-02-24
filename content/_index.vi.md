+++
title = "Workshop Triển Khai Next.js 14 Fullstack Trên AWS EC2 Với CloudFront & DocumentDB"
date = 2025
weight = 1
chapter = false
+++

# Workshop: Triển Khai Next.js 14 Fullstack Trên AWS EC2 Với CloudFront & DocumentDB

### Tổng quan

Trong workshop này, bạn sẽ học cách triển khai một ứng dụng Fullstack NextJS 14 API Routes trên AWS, sử dụng EC2,
DocumentDB, IAM và CloudFront để tối ưu hiệu suất và tối ưu chi phí

![Workshop Architecture](/images/workshop_architecture.png)

### Mục tiêu:

- Hiểu cách tạo và cấu hình EC2, DocumentDB, CloudFront và IAM trên AWS.

- Triển khai ứng dụng NextJS 14 Fullstack trên EC2.

- Kết nối và sử dụng DocumentDB từ EC2, bao gồm cách migration dữ liệu từ MongoDB lên DocumentDB.

- Hiểu cách cấu hình CloudFront làm reverse proxy để tăng tốc API Routes của NextJS lên EC2.

### Yêu cầu:

- Tài khoản AWS với quyền truy cập IAM.

- Một máy tính với SSH client (như Terminal hoặc PuTTY).

### Nội dung

1. [Giới thiệu](1-introduce/)
2. [Hạn chế quyền truy cập với IAM Service](2-restrict-access/)
3. [Khởi tạo VPC](3-create-vpc-instance/)
4. [Khởi tạo EC2 Instance](4-create-ec2-instance/)
5. [Khởi tạo DocumentDB](5-create-documentdb-instance/)
6. [Triển khai ứng dụng lên EC2](6-deploy-the-application-to-ec2/)
7. [Khởi tạo CloudFront](7-create-cloudfront/)
8. [Tính toán chi phí & tối ưu](8-cost-calculation-optimization/)
9. [Dọn dẹp tài nguyên](9-clean-up/)