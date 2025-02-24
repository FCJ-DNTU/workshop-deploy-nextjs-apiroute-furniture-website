+++
title = "Giới thiệu"
date = 2020-05-14T00:38:32+07:00
weight = 1
chapter = false
pre = "<b>1. </b>"
+++

Trong workshop này, bạn sẽ học cách triển khai một ứng dụng Fullstack Next.js 14 với API Routes trên AWS EC2, sử dụng
DocumentDB làm cơ sở dữ liệu. Hướng dẫn từng bước sẽ giúp bạn thiết lập một môi trường AWS hoàn chỉnh, bao gồm **IAM**, **VPC**, **EC2** và **DocumentDB**, đồng thời kiểm tra và triển khai ứng dụng một cách hiệu quả, đảm bảo tính bảo mật và khả năng mở
rộng.

![Workshop Architecture](/images/workshop_architecture.png)

### 🎯 Mục tiêu:

- Hiểu cách tạo và cấu hình **EC2, DocumentDB, CloudFront và IAM trên AWS**.

- Triển khai ứng dụng NextJS 14 Fullstack trên EC2.

- Kết nối và sử dụng DocumentDB từ EC2, bao gồm cách migration dữ liệu từ MongoDB lên DocumentDB.

- Hiểu cách cấu hình CloudFront làm reverse proxy để tăng tốc API Routes của NextJS lên EC2.

### ✍ Yêu cầu:

- Tài khoản AWS với quyền truy cập IAM.

- Một máy tính với SSH client (như Terminal hoặc PuTTY).

### ⚙️ Luồng hoạt động:

**1. Client gửi request**

- Người dùng truy cập website.
- Request được gửi đến CloudFront, đóng vai trò reverse proxy để tăng tốc API Routes của NextJS trên EC2.

**2. CloudFront tiếp nhận request**

- Nếu dữ liệu đã được cache, CloudFront trả về ngay mà không cần gọi EC2.
- Nếu không có cache, CloudFront forward request đến Amazon EC2.

**3. EC2 xử lý request**

- Nếu request cần dữ liệu từ database, EC2 sẽ kết nối đến Amazon DocumentDB.

**4. Truy vấn dữ liệu từ Amazon DocumentDB**

- DocumentDB nằm trong **private subnet** để bảo mật, không thể truy cập trực tiếp từ internet.
- EC2 gửi truy vấn đến Primary DocumentDB trong AZ1.
- Nếu cần dữ liệu chỉ để đọc, DocumentDB có thể lấy từ Replica trong AZ2 để giảm tải cho Primary.

**5. Trả kết quả về client**

- DocumentDB trả dữ liệu cho EC2.
- EC2 xử lý xong và trả kết quả về CloudFront.
- CloudFront có thể cache response để phục vụ nhanh hơn trong lần sau.
- Cuối cùng, response được gửi đến client.

**6. Quản lý quyền truy cập bằng IAM**

- IAM kiểm soát quyền truy cập cho user hoặc dịch vụ.
- IAM User thuộc một IAM Group và tuân theo các chính sách (Policies) để giới hạn quyền truy cập vào tài nguyên AWS như EC2, DocumentDB.
