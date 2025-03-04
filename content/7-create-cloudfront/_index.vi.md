---
title: "Khởi tạo CloudFront"
date: 2025
weight: 7
chapter: false
pre: "<b>7. </b>"
---

Sau khi deploy ứng dụng lên EC2, chúng ta sẽ khởi tạo CloudFront để cải thiện hiệu suất cho website

#### 1. Tạo CloudFront Distribution

- Vào giao diện của [CloudFront](https://us-east-1.console.aws.amazon.com/cloudfront/v4/home#/distributions) và chọn **Create distribution**
  ![create-distribution](/images/7-create-cloudfront/7.create-distribution.png)

- Trong **Create distribution**, chọn những mục sau:

  - **Origin domain**: your-public-ipv4-dns-ec2
  - **Protocol**: HTTP only
  - **HTTP Port**: 80
  - **Viewer protocol policy**: HTTP and HTTPS
  - **Allowed HTTP methods**: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
  - **Web Application Firewall (WAF)**: Do not enable security protections
  - Sau đó chọn **Create distribution** để tạo distribution
    ![create-distribution-1](/images/7-create-cloudfront/7.1.png)
    ![create-distribution-2](/images/7-create-cloudfront/7.2.png)
    ![create-distribution-3](/images/7-create-cloudfront/7.3.png)

- Sẽ mất khoảng 5-10 phút để tạo, khi tạo thành công bạn sẽ thấy **Last modified**
  ![create-success](/images/7-create-cloudfront/7.4.png)

#### 2. Tạo Behavior Route API

- Trong distribution vừa mới tạo, chọn tab **Behaviors**
- Chọn **Create Behavior**
  ![behavior](/images/7-create-cloudfront/7.behavior.png)

- Trong **Create behavior**, điền những thông tin sau:

  - **Path pattern**: `/api/*`
  - **Origin**: Chọn EC2 mà bạn đã tạo
  - **Viewer Protocol Policy**: Redirect HTTP to HTTPS
  - **Allowed HTTP Methods**: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
  - **Cache Policy**: CachingDisabled (để API không bị cache)
    ![create-behavior](/images/7-create-cloudfront/7.create-behavior.png)

- Nhấn **Create behavior**

#### 3. Cập nhật lại API URL trong file .env

- Kết nối tới EC2 thông qua **EC2 Instance Connect**
- Gõ lệnh sau để vào file **.env** để chỉnh sửa
  ```shell
  cd e-commerce-furniture
  nano .env
  ```
- Trong **.env**, chỉnh lại **NEXT_PUBLIC_API_URL** thành **Distribution domain name** của **CloudFront**, sau đó ấn **Ctrl + X -> Y -> Enter** để lưu
  ![change-api-url](/images/7-create-cloudfront/7.change-api-url.png)

- Sau khi thay đổi file .env thì gõ lại lệnh **npm run build** để build lại project

  ```shell
  npm run build
  ```

- Sau khi build xong thì **restart lại pm2**
  ```shell
  pm2 restart all
  ```

#### 4. Cài đặt Nginx

Lý do sử dụng Nginx trong workshop này:

**Reverse Proxy:**

- Ban đầu, ứng dụng chạy trên EC2 với cổng 3000, Nginx giúp chuyển hướng (proxy_pass) từ cổng 80 hoặc 443 sang 3000, giúp ứng dụng có thể truy cập bằng tên miền hoặc CloudFront mà không cần chỉ định cổng
- Ví dụ, sau khi cài đặt, bạn chỉ cần truy cập: **http://your-ip-ec2 (Không cần :3000)**. Nginx sẽ tự động chuyển request về localhost:3000 (nơi ứng dụng của bạn chạy)

- Cài đặt **Nginx**

  ```shell
  sudo apt update
  sudo apt install nginx -y
  ```

- Cấu hình Nginx để chuyển hướng CloudFront

  - Sửa file config của Nginx:

    ```shell
    sudo nano /etc/nginx/sites-available/default
    ```

  - Thêm đoạn sau vào:

    ```shell
    server {
        listen 80;
        server_name _;

        location / {
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
    ```

  - Lưu lại **(Ctrl + X → Y → Enter)**

- Kiểm tra có lỗi cấu hình không

  ```shell
  sudo nginx -t
  ```

- Nếu không có lỗi, restart Nginx:

  ```shell
  sudo systemctl restart nginx
  ```

#### 5. Kiểm tra

- Copy **Distribution domain name** và dán vào một tab mới
  ![test](/images/7-create-cloudfront/7.test.png)

Chúng ta có thể so sánh hiệu suất giữa EC2(bên trái) và CloudFront(bên phải) -> CloudFront đã tối ưu hiệu suất hơn EC2
| ![EC2 Test](/images/7-create-cloudfront/7.ec2.png) | ![CloudFront Test](/images/7-create-cloudfront/7.cloudfront-test2.png) |
|----------------------------------------------------|----------------------------------------------------|

{{< center>}}

### **Hoàn thành! 🚀**

{{< /center>}}
