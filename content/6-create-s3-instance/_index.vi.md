---
title: "Khởi tạo S3 Instance"
date: 2025
weight: 6
chapter: false
pre: "<b>6. </b>"
---

Tiếp theo, để lưu trữ hình ảnh cho ứng dụng chúng ta sẽ sử dụng dịch vụ lưu trữ AWS S3

#### Simple Storage Service (Amazon S3)

**Amazon Simple Storage Service (Amazon S3)** là một dịch vụ lưu trữ đối tượng cung cấp khả năng mở rộng, tính sẵn sàng của dữ liệu, bảo mật và hiệu suất hàng đầu.

#### Khởi tạo dịch vụ lưu trữ S3

1. Truy cập [AWS S3](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1#)

2. Chọn **Create bucket**

- Chọn **AWS Region** - **Asia Pacific (Singapore) ap-southeast-1**
- Đặt tên cho **S3 bucket**, lưu ý tên bucket phải unique
- Vì chúng ta muốn hình ảnh có thể serve cho client => Bỏ chọn **Block all public acccess**, sau đó tích chọn confirm
  ![create-bucket.png](/images/6-create-s3-instance/create-bucket.png)
- Chọn **create bucket**

3. Truy cập S3 bucket

- Khi quá trình tạo hoàn thành, truy cập và kiểm tra thông tin của S3 bucket
  ![s3-bucket.png](/images/6-create-s3-instance/s3-bucket.png)

4. Upload một object vào S3 Bucket

- Truy cập vào S3 bucket, chọn **Upload**
- Kéo thả một file vào khung **Upload**, và chọn **Upload**
  ![upload-object.png](/images/6-create-s3-instance/upload-object.png)

5. Tạo IAM role cho EC2
   {{% notice note %}}
   Best practices. Thay vì kết nối với S3 bằng **Access Key, Secret Key**. Để tăng tính bảo mật, hãy sử dụng **IAM Role** để ủy quyền cho một **EC2 Instance** kết nối tới **S3 Bucket**
   {{% /notice %}}

- Đầu tiên, bạn cần tạo một **role** cho **EC2 Service** kết nối tới **S3 bucket**
- Truy cập [Roles](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/roles), tạo **Create Role**
- Tại giao diện **Select trusted entity**, chọn **entity type** là **AWS Service**
- Use case **EC2**. **Next** để tiếp tục
  ![iam-role.png](/images/6-create-s3-instance/iam-role.png)
- Tiếp theo chọn **AmazonS3FullAccess**, chọn **Next** ( Ở đây, để bảo mật hơn, bạn hoàn toàn có thể sử dụng Custom Policy)
  ![policy.png](/images/6-create-s3-instance/policy.png)
- Tiếp theo, đặt **Role name**, **Description**, và review **Role Details**

6. Gán role cho EC2 service

- Truy cập [EC2 instances](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Instances:)
- Chọn EC2 Instance chúng ta đã khởi tạo
- Chọn **Actions** > **Security** > **Modify IAM Role**
  ![attach-role.png](/images/6-create-s3-instance/attach-role.png)
- Chọn IAM Role vừa tạo và **Update IAM Role**
  ![update-iam-role.png](/images/6-create-s3-instance/update-iam-role.png)

7. Kiểm tra kết nối EC2 với S3

- Truy cập EC2 instance `$ aws s3 ls s3://my-bucket-name`

![ec2-to-s3.png](/images/6-create-s3-instance/ec2-to-s3.png)
