---
title: "Khởi tạo AWS DocumentDB"
date: 2025
weight: 5
chapter: false
pre: "<b>5. </b>"
---

#### AWS Relational Database Service

**Amazon Relational Database Service (Amazon RDS)** là một dịch vụ web giúp dễ dàng thiết lập, vận hành và mở rộng cơ sở
dữ liệu quan hệ trong môi trường đám mây AWS.

#### Tạo một DB Instance trên AWS

{{% notice info %}}
Lưu ý: Trong thủ tục dưới đây, tùy chọn Standard create được bật và Easy create không được bật. Thủ tục này sử dụng
MySQL làm ví dụ.
{{% /notice %}}

#### Để tạo một DB Instance:

1. Đăng nhập vào **AWS Management Console** và mở Amazon RDS console tại https://console.aws.amazon.com/rds/.

2. Ở góc trên bên phải của **Amazon RDS** console, chọn khu vực AWS mà bạn muốn tạo DB Instance.

3. Trong khung điều hướng, chọn **Databases**.

4. Chọn **Create database**, sau đó chọn **Standard create**.

![rds-interface.png](/images/5-create-rds-instance/rds-interface.png)

5. Đối với **Engine type**, chọn MariaDB, Microsoft SQL Server, MySQL, Oracle, hoặc PostgreSQL. Trong ví dụ này, chúng
ta sử dụng **MySQL**.

6. Đối với **Edition**, chọn **MySQL Community**

7. Đối với Version, chọn phiên bản của engine. e.g. MySQL 8.0.39
![create-db.png](/images/5-create-rds-instance/create-db.png)

8. Trong phần Templates, chọn **Free tiers** template:

9. Để nhập mật khẩu chính của bạn, làm theo các bước sau:

- Trong phần Settings, mở Credential Settings.
- Nếu bạn muốn chỉ định một mật khẩu, hãy bỏ chọn hộp kiểm Auto generate a password nếu nó đã được chọn.
- (Tùy chọn) Thay đổi giá trị Master username.
- Nhập cùng mật khẩu trong Master password và Confirm password.
- (Tùy chọn) Cài đặt kết nối với một tài nguyên tính toán cho DB Instance này.

![settings.png](/images/5-create-rds-instance/settings.png)

10. Bạn có thể cấu hình kết nối giữa một Amazon EC2 instance và DB Instance mới trong quá trình tạo DB Instance.

- Trong phần Connectivity, chọn **Connect to EC2 Compute Resource**
- Chọn **EC2 Instance** chúng ta vừa tạo

11. Cấu hình **DB Subnet Group**

- Tại phần **DB Subnet Group**, chọn **Choose Existing** chọn **golang-db-subnet-group**

12. Cấu hình **VPC Security Group**

- Chọn **Choose Existing**, tại dropdown chọn **private-sg**, security group chúng ta vừa khởi tạo

![connectivity.png](/images/5-create-rds-instance/connectivity.png)

13. Chọn Create database.

14. Kiểm tra RDS

- Trong trang chi tiết của instance RDS, bạn có thể tìm thấy các thông tin liên quan đến kết nối như Endpoint (điểm kết
nối), Port (cổng), và Username (tên người dùng).
- Điểm kết nối (Endpoint) là URL hoặc địa chỉ IP mà bạn sử dụng để kết nối tới cơ sở dữ liệu RDS.

![rds.png](/images/5-create-rds-instance/rds.png)

15. Kiểm tra kết nối từ EC2 tới MySQL

```shell
$ sudo yum install mysql
$ mysql -h <endpoint> -P 3306 -u admin -p <password>
            # $ mysql -h mysql-golang-db.c1a20mqwgeb9.ap-southeast-1.rds.amazonaws.com -P 3306 -u admin -pAdmin123
            ```

            ![ec2-to-mysql.png](/images/5-create-rds-instance/ec2-to-mysql.png)

            16. Tạo Database
            {{% notice tip %}}
            Admin user không thể truy cập trực tiếp với database **mysql**, chúng ta nên tạo database mới để truy cập.
            Mình đã troubleshoot lỗi này trong một buổi, và đơn giản là do privileges bên trong RDS :D
            {{% /notice %}} >Since RDS is a managed service, to maintain the system integrity and stability, super user
            privileges are not provided even to the master user of the DB instance, and therefore, such error message is
            expected, as the RDS MySQL master user by default does not have the ADMIN, ROLE_ADMIN, SUPER privileges.

            ```mysql
            CREATE DATABASE blog_db;
            # Query OK, 1 row affected (0.01 sec)
            ```

            17. Truy cập vào Database
            ![db.png](/images/5-create-rds-instance/db.png)