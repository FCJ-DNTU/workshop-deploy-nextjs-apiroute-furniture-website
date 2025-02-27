---
title: "Khởi tạo AWS DocumentDB"
date: 2025
weight: 5
chapter: false
pre: "<b>5. </b>"
---

![aws-documentdb.png](/images/5-create-documentdb-instance/aws-documentdb.png)

#### AWS DocumentDB

**Amazon DocumentDB** (với khả năng tương thích MongoDB) là **cơ sở dữ liệu tài liệu JSON gốc** được quản lý đầy đủ, giúp vận hành khối lượng công việc tài liệu quan trọng ở hầu hết mọi quy mô mà không cần quản lý cơ sở hạ tầng một cách dễ dàng và tiết kiệm chi phí.

#### Lợi ích khi sử dụng DocumentDB:

- **Tương thích với MongoDB:** Hỗ trợ các API MongoDB phổ biến, dễ dàng di chuyển dữ liệu từ MongoDB.
- **Hiệu suất cao & Mở rộng linh hoạt:** Kiến trúc lưu trữ phân tán giúp tăng tốc độ đọc/ghi và tự động mở rộng khi cần thiết.
- **Quản lý dễ dàng:** Là dịch vụ **fully managed**, giúp giảm tải quản trị cơ sở dữ liệu.

#### 💰 Giá của Amazon DocumentDB

Giá của **Amazon DocumentDB** phụ thuộc vào các yếu tố sau:

- **On-Demand Instances** – Tính theo giờ sử dụng của từng loại instance.
- **Database I/O** – Tính theo số lần đọc/ghi dữ liệu (theo triệu I/Os).
- **Database Storage** – Tính theo dung lượng lưu trữ (GB/tháng).
- **Backup Storage** – Tính theo dung lượng backup vượt mức database storage (GB/tháng).

AWS cung cấp **hai tùy chọn** cấu hình giá:

- **Amazon DocumentDB Standard (Trả tiền theo I/O sử dụng)**
  - Phù hợp với workload có I/O thấp đến trung bình.
  - Tính phí dựa trên cả 4 yếu tố: Instance, Database I/O, Storage, và Backup.
  - Nếu chi phí I/O thấp hơn 25% tổng chi phí, đây là lựa chọn phù hợp.
- **Amazon DocumentDB I/O-Optimized (Đã bao gồm phí I/O trong giá thuê instance)**
  - Phù hợp với ứng dụng có I/O cao, cần tính toán chi phí ổn định.
  - Chỉ tính phí trên 3 yếu tố: Instance, Storage, và Backup.
  - Không tính phí I/O riêng biệt, giúp dự đoán chi phí dễ dàng hơn.
  - Nếu chi phí I/O chiếm trên 25% tổng chi phí, đây là lựa chọn phù hợp.

#### Workshop này nên chọn cấu hình nào?

- Chúng ta sẽ chọn **Amazon DocumentDB Standard** để tiết kiệm chi phí, vì:
  - Khối lượng truy vấn ban đầu thấp.
  - Chỉ trả tiền cho I/O thực tế sử dụng.
  - Linh hoạt nâng cấp lên I/O-Optimized khi hệ thống có nhiều truy vấn hơn.

#### Tạo một DB Instance trên AWS

1. Đăng nhập vào **AWS Management Console** và mở **Amazon DocumentDB**

2. Tạo **DocumentDB Cluster**
   ![Cluster-interface.png](/images/5-create-documentdb-instance/5.1.png)

3. Trong **Create Amazon DocumentDB cluster**, điền thông tin sau:

   - **Cluster type**: Instance-based cluster
   - **Cluster identifier**: `docdb-nextjs-workshop`
   - **Engine version**: Chọn phiên bản mới nhất
   - **Cluster storage configuration**: Amazon DocumentDB Standard
   - **Instance class**: db.t3.medium
   - **Number of instances**: 2 (1 Primary + 1 Replica)
   - **Connectivity**: Connect to an EC2 compute resource
   - **EC2 Instance**: Chọn EC2 đã tạo
   - **Username**: `user123`
   - **Password**: `user1234`
   - **Subnet group**: chọn subnet group đã tạo ở **3.3**
   - **VPC security groups**: **private-sg-documentdb (VPC)**
   - **Deletion protection**: bỏ tích **Enable deletion protection**
   - Kiểm tra lại các thiết lập và nhấn **Create cluster**
     ![create-1.png](/images/5-create-documentdb-instance/5.2.png)
     ![create-2.png](/images/5-create-documentdb-instance/5.3.png)
     ![create-3.png](/images/5-create-documentdb-instance/5.4.png)
     ![create-4.png](/images/5-create-documentdb-instance/5.5.png)
     ![create-5.png](/images/5-create-documentdb-instance/5.6.png)
     ![create-6.png](/images/5-create-documentdb-instance/5.7.png)
   - Tạo **Cluster** thành công
     ![create-success.png](/images/5-create-documentdb-instance/5.8.png)

4. Kiểm tra kết nối từ EC2 tới DocumentDB

- Vào cluster vừa mới tạo, chọn tab **Configuration** và copy **Cluster endpoint**
  ![copy-cluster.png](/images/5-create-documentdb-instance/5.9.png)
- Vào EC2, ấn nút **Connect** và ấn nút **Connect** trong tab **EC2 Instance Connect**

  ```shell
  $ sudo apt-get install -y netcat
  $ nc -zv docdb-nextjs-workshop.cluster-c10k88ou8amc.ap-southeast-1.docdb.amazonaws.com 27017
  ```

  ![success.png](/images/5-create-documentdb-instance/5.10.png)

{{< center>}}

### **Hoàn thành! 🚀**

{{< /center>}}
