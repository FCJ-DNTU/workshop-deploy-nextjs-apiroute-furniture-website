---
title: "Khởi tạo EC2 Instance"
date: 2025
weight: 4
chapter: false
pre: "<b>4. </b>"
---

#### Amazon EC2

**Amazon Elastic Compute Cloud (Amazon EC2)** cung cấp khả năng tính toán có thể mở rộng và on-demand trong môi trường Cloud của Amazon Web Services (AWS).

![ec2](https://docs.aws.amazon.com/images/AWSEC2/latest/UserGuide/images/instance-types.png)

Dưới đây là nội dung mở rộng cho phần "Trường hợp sử dụng chính của EC2 trong Workshop":

#### Trường hợp sử dụng chính của EC2 trong Workshop

Trong bài Workshop này, **Amazon EC2** được sử dụng để triển khai một ứng dụng Golang đơn giản. Với mục tiêu tiết kiệm chi phí và tối ưu tài nguyên, chúng ta sử dụng loại instance **t2.micro**. Đây là loại instance phù hợp cho các ứng dụng nhỏ hoặc môi trường phát triển, nhờ cung cấp mức tài nguyên cơ bản với chi phí thấp.

#### Lý do chọn **t2.micro**:

- **Chi phí thấp:** Là một loại instance thuộc Free Tier (miễn phí trong giới hạn AWS Free Tier), phù hợp cho việc học tập hoặc thử nghiệm.
- **Tài nguyên cơ bản:** Bao gồm 1 vCPU và 1GB RAM, đủ để chạy ứng dụng Golang đơn giản hoặc thử nghiệm các dịch vụ AWS cơ bản.
- **Burstable Performance:** Có khả năng tăng cường hiệu năng khi cần, rất hữu ích cho các ứng dụng có tải không đều.

---

#### Tạo một EC2 Instance trên AWS

Bạn có thể tạo một instance Linux bằng cách sử dụng AWS Management Console theo hướng dẫn sau đây.
Hướng dẫn này được thiết kế để giúp bạn tạo instance đầu tiên một cách nhanh chóng, nên nó không bao gồm tất cả các tùy chọn có thể có.

Để biết thông tin về các tùy chọn nâng cao, hãy xem Hướng dẫn tạo instance bằng cách sử dụng hướng dẫn mới về Launch Instance. Để biết thông tin về cách khác để tạo instance của bạn, hãy xem Hướng dẫn tạo instance của bạn.

#### 1. Truy cập AWS Console

- Mở trình duyệt và truy cập vào Amazon EC2 console tại https://console.aws.amazon.com/ec2/.

#### 2. Chọn Launch instance

- Tại màn hình dashboard của EC2 console, trong hộp Launch instance, chọn Launch instance, sau đó chọn Launch instance từ các tùy chọn xuất hiện.

#### 3. Đặt tên cho instance

- Dưới phần Name and tags, cho Name, nhập tên mô tả cho instance của bạn.

#### 4. Chọn image (Amazon Machine Image - AMI)

- Dưới phần Application and OS Images (Amazon Machine Image), làm theo các bước sau đây:
- Chọn Quick Start, sau đó chọn Amazon Linux. Đây là hệ điều hành (OS) cho instance của bạn.
- Từ Amazon Machine Image (AMI), chọn một phiên bản HVM của Amazon Linux 2023. Lưu ý rằng các AMI này được đánh dấu là Free tier eligible. Một Amazon Machine Image (AMI) là một cấu hình cơ bản dùng làm mẫu cho instance của bạn.
  ![ami.png](/images/4-create-ec2-instance/ami.png)

#### 5. Chọn loại instance

- Dưới phần Instance type, từ danh sách Instance type, bạn có thể chọn cấu hình phần cứng cho instance của bạn. Chọn loại instance t2.micro, mà mặc định đã được chọn. Loại instance t2.micro đủ điều kiện để sử dụng miễn phí trong AWS Free Tier. Tại các khu vực mà loại t2.micro không có sẵn, bạn có thể sử dụng loại t3.micro trong AWS Free Tier. Để biết thêm thông tin, hãy xem AWS Free Tier.

#### 6. Chọn Key Pair

- Dưới phần Key pair (login), cho Key pair name, chọn key pair mà bạn đã tạo khi cài đặt.

{{% notice warning %}}
Cảnh báo: Đừng chọn Proceed without a key pair (Không được khuyến nghị). Nếu bạn tạo instance mà không có key pair, bạn sẽ không thể kết nối vào nó.
{{% /notice %}}

#### 7. Cấu hình Security Group

- Bên cạnh Network settings, chọn Edit. Đối với Security group name, bạn sẽ thấy rằng hướng dẫn đã tạo và chọn một security group cho bạn. Bạn chọn security group mà bạn đã tạo khi cài đặt bằng các bước sau đây:

- Chọn Select existing security group.
  - Từ Common security groups, chọn security group của bạn từ danh sách các security group hiện có.
  - Xác nhận và khởi động instance
  - Giữ nguyên các lựa chọn mặc định cho các cài đặt khác của instance của bạn.
  - Xem lại bản tóm tắt về cấu hình instance của bạn trong Summary panel, và khi bạn đã sẵn sàng, chọn Launch instance.

![sg-config.png](/images/4-create-ec2-instance/sg-config.png)

#### 8. Xác nhận và kiểm tra

- Một trang xác nhận sẽ thông báo rằng instance của bạn đang được khởi động. Chọn **View all instances** để đóng trang xác nhận và trở lại giao diện console.
- Trên màn hình Instances, bạn có thể xem trạng thái của quá trình khởi động. Có một thời gian ngắn để instance được khởi động. Khi bạn khởi động instance, trạng thái ban đầu của nó là pending. Sau khi instance khởi động, trạng thái của nó sẽ thay đổi thành running và nó sẽ nhận được một tên DNS công cộng. Nếu cột Public IPv4 DNS bị ẩn, hãy chọn biểu tượng cài đặt (Biểu tượng cài đặt) ở góc trên bên phải, bật Public IPv4 DNS và chọn Confirm.
- Có thể mất vài phút để instance sẵn sàng để bạn kết nối vào. Hãy kiểm tra xem instance của bạn đã vượt qua kiểm tra trạng thái; bạn có thể xem thông tin này trong cột Status check.

![review.png](/images/4-create-ec2-instance/review-png.png)

- Kiểm tra Public IPv4 click chọn vào **EC2 Instance** > **Networking** tab > **Public IPv4 Address**

  ![check-ipv4.png](/images/4-create-ec2-instance/check-ipv4.png)

#### Kiểm tra kết nối tới EC2 Instance

#### 1. Kết nối EC2 Instance bằng SSH

- Chọn EC2 Instance vừa khởi tạo, chọn **Connect**
- Tại giao diện **Connect to instance**, chọn tab **SSH Client**

![ssh-ec2.png](/images/4-create-ec2-instance/ssh-ec2.png)

- Nếu bạn đang sử dụng Windows, hãy cài đặt [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install) để thực hiện các câu lệnh Linux (hoặc có thể sử dụng ứng dụng [Putty](https://www.putty.org/))
- Nếu bạn đang sử dụng MacOS, copy example ssh command line và paste vào **Terminal**, câu lệnh có cú pháp:

```
$ chmod 400 "key-pair.pem"
$ ssh -i path/to/key-pair.pem ec2-user@domain
```

#### 2. Truy cập thành công

![complete.png](/images/4-create-ec2-instance/complete.png)
