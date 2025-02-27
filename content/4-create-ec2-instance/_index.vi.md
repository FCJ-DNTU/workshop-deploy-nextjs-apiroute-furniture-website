---
title: "Khởi tạo EC2 Instance"
date: 2025
weight: 4
chapter: false
pre: "<b>4. </b>"
---

#### Amazon EC2

**Amazon Elastic Compute Cloud (Amazon EC2)** là một cơ sở hạ tầng điện toán đám mây được cung cấp bởi Amazon Web
Services (AWS) giúp cung cấp tài nguyên máy tính ảo hoá theo yêu cầu.

#### Amazon EC2 Instance

**Amazon EC2 Instance** là một cloud server. Với một tài khoản bạn có thể tạo và sử dụng nhiều Amazon EC2 Instance. Các
Amazon EC2 Instance được chạy trên cùng một server vật lý và chia sẻ memory, CPU, ổ cứng...

#### Mô hình triển khai EC2 trong Workshop

- Trong Workshop này, chúng ta sẽ khởi tạo EC2 Instance để chạy ứng dụng Next.js Fullstack với API Routes. EC2 sẽ đóng
vai trò như một Application Server xử lý cả Frontend và Backend.

#### Lý do chọn **t2.medium**:

- **Cân bằng giữa hiệu suất và chi phí:**

- t2.medium cung cấp 2 vCPU và 4GB RAM, đủ để chạy Next.js Fullstack (bao gồm cả API Routes).
- Giá thuê t2.medium thấp hơn so với các loại instance cao cấp như t3.large hoặc m5.large, giúp tiết kiệm chi phí.

- **Hỗ trợ Burstable Performance:** T2 instances là Burstable Performance Instances, tức là có thể tăng hiệu suất CPU
khi cần thiết.

- **Linh hoạt trong mở rộng (Scaling):** Nếu cần mở rộng, có thể dễ dàng chuyển sang t3.medium hoặc m5.large mà không
cần thay đổi kiến trúc.

**Trong workshop này, chúng ta chọn tính tiền t2.medium theo On-Demand ở region ap-southeast-1 (Singapore)**
| On-Demand hourly rate | vCPU | Memory |
| ----------------------- | -----| -------|
| $0.0584 | 2 | 4 GiB |

**Tại sao chọn On-Demand Instances:** Trả tiền theo nhu cầu sử dụng mà không cần cam kết dài hạn

**Chi phí dự kiến khi dùng On-Demand:**

- t2.medium (2 vCPU, 4GB RAM): **0.0584 USD/giờ**
- Nếu chạy liên tục 24/7 (730 giờ/tháng): **~42.63 USD/tháng**

**👉 Tối ưu chi phí:** Chỉ bật EC2 khi cần sử dụng, tắt khi không cần.

---

#### Khởi tạo EC2 Instance

#### 1. Truy cập AWS Console

- Mở trình duyệt và truy cập vào Amazon EC2 console tại https://console.aws.amazon.com/ec2/.

#### 2. Trong **Dashboard**, chọn **Launch instance**

![Launch.png](/images/4-create-ec2-instance/4.1.png)

#### 3. Đặt tên cho instance

- Trong phần **Name and tags**:
- **Name:** `nextjs-ec2`

#### 4. Chọn image (Amazon Machine Image - AMI)

- Dưới phần Application and OS Images (Amazon Machine Image), làm theo các bước sau đây:
- Chọn **Quick Start**, sau đó chọn **Ubuntu**. Đây là hệ điều hành (OS) cho instance của bạn.
- Từ Amazon Machine Image (AMI), chọn **Ubuntu 24.04 LTS HVM**. Lưu ý rằng AMI này được đánh dấu là **Free tier
eligible**. Một Amazon Machine Image (AMI) là một cấu hình cơ bản dùng làm mẫu cho instance của bạn.
![ami.png](/images/4-create-ec2-instance/4.2.png)

#### 5. Chọn loại instance

- Trong Instance type, chọn loại instance **t2.medium** và dưới phần **Key pair (login)**, chọn **Create new key pair**
![instance.png](/images/4-create-ec2-instance/4.3.png)

#### 6. Tạo Key Pair

- Trong **Create key pair**, điền những thông tin sau:
- **Key pair name**: `nextjs-kp`
- **Key pair type**: chọn **RSA**
- **Private key file format**: **.pem**
- Sau đó chọn **Create key pair**. Một file **.pem** sẽ tự động tải về máy
![instance.png](/images/4-create-ec2-instance/4.4.png)

{{% notice warning %}}
Cảnh báo: Đừng chọn Proceed without a key pair (Không được khuyến nghị). Nếu bạn tạo instance mà không có key pair, bạn
sẽ không thể kết nối vào nó.
{{% /notice %}}

#### 7. Cấu hình Security Group

- Trong **Network settings**, chọn **Edit** ở bên cạnh. Đối với Security group name, bạn đã tạo và chọn một security
group cho EC2. Bạn chọn security group mà bạn đã tạo khi cài đặt bằng các bước sau đây:

- Chọn **Select existing security group**.
- Từ **Common security groups**, chọn security group của bạn từ danh sách các security group hiện có.
- Xác nhận và khởi động instance
- Giữ nguyên các lựa chọn mặc định cho các cài đặt khác của instance của bạn.
- Xem lại bản tóm tắt về cấu hình instance của bạn trong **Summary**, và khi bạn đã sẵn sàng, chọn **Launch instance**.

![sg-config.png](/images/4-create-ec2-instance/4.6.png)

#### 8. Xác nhận và kiểm tra

- Một trang thông báo rằng instance của bạn đang được khởi động. Chọn **View all instances** để quay lại giao diện
console..
![view.png](/images/4-create-ec2-instance/4.7.png)

- Trên màn hình **Instances**, kiểm tra trạng thái EC2:
- **pending** → đang khởi động
- **running** → đã khởi động thành công, có Public DNS
- Đợi vài phút để EC2 sẵn sàng, kiểm tra **Status check** để đảm bảo hoạt động ổn định.
- Xem Public IPv4 tại **EC2 Instance** > **Networking** > **Public IPv4 Address**.
![review.png](/images/4-create-ec2-instance/4.8.png)

#### 9. Kết nối bằng EC2 Instance

- Chọn EC2 Instance vừa khởi tạo, chọn **Connect**
![connect.png](/images/4-create-ec2-instance/4.9.png)
- Tại giao diện **Connect to instance**, chọn tab **EC2 Instance Connect** và chọn **Connect**
![ssh-client.png](/images/4-create-ec2-instance/connect-ec2.png)

- Kết nối thành công
![success.png](/images/4-create-ec2-instance/connect-successfully.png)

{{< center>}}

  ### **Hoàn thành! 🚀**

  {{< /center>}}