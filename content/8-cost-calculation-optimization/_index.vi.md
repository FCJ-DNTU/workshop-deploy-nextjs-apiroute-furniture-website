---
title: "Tính toán chi phí và tối ưu"
date: 2025
weight: 8
chapter: false
pre: "<b>8. </b>"
---

#### 1. Chi phí của từng dịch vụ

- **Amazon VPC**: Amazon VPC không tính phí tạo và sử dụng các thành phần cơ bản như VPC, subnet, ACL, security group. Tuy nhiên, việc sử dụng địa chỉ IP công cộng hoặc Elastic IP có thể phát sinh chi phí

- **AWS IAM:** IAM được cung cấp miễn phí. Có thể tạo và quản lý người dùng, nhóm và quyền truy cập mà không phải chịu thêm chi phí

- **Amazon EC2** với phiên bản t2.medium:

  - Phiên bản t2.medium cung cấp 2 vCPU và 4 GB RAM
  - Chi phí theo nhu cầu (On-Demand) cho phiên bản này trong khu vực ap-southeast-1 khoảng $0.0464 mỗi giờ

- **Amazon DocumentDB** (phiên bản db.t3.medium):

  - Phiên bản db.t3.medium cung cấp 2 vCPU và 4 GB RAM
  - Chi phí theo nhu cầu cho phiên bản này trong khu vực **ap-southeast-1** khoảng $0.078 mỗi giờ
  - Tổng chi phí hàng tháng (720 giờ): 720 giờ x $0.078/giờ = ~$56.16
  - Lưu trữ: $0.10/GB-tháng
  - Sao lưu: $0.02/GB-tháng

- **Amazon CloudFront**:

  - Chi phí phụ thuộc vào lưu lượng dữ liệu truyền tải và số lượng yêu cầu
  - Ví dụ, chi phí truyền dữ liệu ra ngoài (Data Transfer Out) cho 10 TB đầu tiên mỗi tháng là $0.085/GBs
  - Số lượng yêu cầu HTTP/HTTPS: $0.0075 cho mỗi 10,000 yêu cầu

Đây là bảng chi phí ước tính cho từng dịch vụ AWS trong khu vực **ap-southeast-1**:

| **Dịch vụ**    | **Loại Instance**           | **Chi phí hàng ngày ($)** | **Chi phí hàng tháng ($)** |
| -------------- | --------------------------- | ------------------------- | -------------------------- |
| **EC2**        | t2.medium (2 vCPU, 4GB RAM) | 1.11 ($0.0464 x 24h)      | 33.41 ($0.0464 x 720h)     |
| **DocumentDB** | db.t3.medium (2 vCPU, 4GB)  | 1.87 ($0.078 x 24h)       | 56.16 ($0.078 x 720h)      |
| **CloudFront** | 100GB Data Transfer         | 8.50 ($0.085 x 100GB)     | 255.00 ($0.085 x 3TB)      |
| **VPC**        |                             | Miễn phí                  | Miễn phí                   |
| **IAM**        |                             | Miễn phí                  | Miễn phí                   |

#### 2. Tối ưu chi phí

**2.1. Giám sát và quản lý chi phí**

- **Dùng AWS Cost Explorer**: Theo dõi chi phí theo thời gian thực để kiểm soát mức sử dụng tài nguyên
- **Thiết lập Billing Alerts**: Đặt cảnh báo qua AWS Budgets để nhận thông báo khi chi phí vượt quá ngưỡng quy định
- **Tận dụng AWS Free Tier**: Nếu bạn mới sử dụng AWS, hãy tận dụng các dịch vụ miễn phí trong 12 tháng đầu tiên

**2.2. Tối ưu chi phí EC2**

- **Chọn loại instance phù hợp**: Nếu tải không quá cao, có thể sử dụng **t3.medium** thay vì t2.medium, vì t3 có hiệu suất tốt hơn với giá tương đương
- **Dùng Spot Instances**: Nếu workload không cần chạy liên tục, bạn có thể sử dụng **Spot Instances** để giảm đến 90% chi phí so với On-Demand
- **Sử dụng Auto Scaling**: Thiết lập **Auto Scaling Group** để tăng giảm số lượng EC2 theo nhu cầu thực tế, tránh lãng phí tài nguyên
- **Dùng Reserved Instances / Savings Plans**: Nếu có thể cam kết sử dụng EC2 trong 1 hoặc 3 năm, dùng Reserved Instances hoặc Savings Plans giúp tiết kiệm đến 72% so với On-Demand
- **Tắt máy chủ khi không cần thiết**: Khi nào cần dụng EC2 thì chúng ta bật, còn khi không cần dùng thì có thể tắt để tiết kiệm chi phí

**2.3. Tối ưu chi phí Amazon DocumentDB**

- **Giảm số lượng Replica**: DocumentDB có thể có nhiều Replica để tăng hiệu suất đọc, nhưng nếu không cần thiết, hãy chỉ sử dụng **1 primary instance** để tiết kiệm chi phí
- **Sử dụng Auto Scaling Storage**: DocumentDB tự động mở rộng dung lượng lưu trữ, nhưng bạn nên giám sát và xóa dữ liệu không cần thiết để tránh phát sinh chi phí không cần thiết
- **Sao lưu hợp lý**: Amazon DocumentDB lưu trữ **backups** miễn phí bằng với dung lượng lưu trữ chính. Tuy nhiên, nếu sao lưu nhiều hơn, bạn sẽ bị tính phí **$0.02/GB-tháng**, vì vậy nên giữ số lượng backup hợp lý
- **Dùng Reserved Instances**: Nếu bạn chắc chắn sử dụng DocumentDB lâu dài, có thể đặt trước Reserved Instances để tiết kiệm lên đến 60%

**2.4. Tối ưu chi phí Amazon CloudFront**

- **Bật Compression và Caching**: Bật **Gzip/Brotli** Compression để giảm dung lượng truyền tải dữ liệu, giúp tiết kiệm băng thông
- **Sử dụng Regional Edge Caches**: CloudFront có thể cache nội dung gần người dùng hơn, giảm tải trên server gốc và tiết kiệm chi phí truyền dữ liệu
- **Giảm request không cần thiết**: Nếu trang web có nhiều file tĩnh (CSS, JS, hình ảnh), nên tối ưu bằng cách giảm số request hoặc dùng Lazy Load để tải nội dung khi cần thiết
- **Sử dụng CloudFront Free Tier**: AWS cung cấp 1TB miễn phí hàng tháng trong năm đầu tiên. Nếu trang web chưa có quá nhiều traffic, bạn có thể tận dụng Free Tier để giảm chi phí

**2.5. Tối ưu chi phí VPC và IAM**

- **Hạn chế Elastic IP**: Elastic IP chỉ miễn phí khi gán cho một instance đang chạy. Nếu không sử dụng, hãy xóa để tránh bị tính phí **$0.005/giờ**
- **Xóa tài nguyên không sử dụng**: Kiểm tra và xóa **subnet, security group, NAT Gateway, VPN Gateway** nếu không còn sử dụng để tránh phát sinh chi phí không cần thiết
- **Quản lý quyền truy cập IAM**: Hạn chế tạo quá nhiều IAM Users và Roles, chỉ cấp quyền khi cần thiết để tránh rủi ro bảo mật
