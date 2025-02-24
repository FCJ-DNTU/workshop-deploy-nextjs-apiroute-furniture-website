---
title: "Hạn chế quyền truy cập với IAM Service"
date: 2025
weight: 2
chapter: false
pre: "<b>2. </b>"
---

{{% notice note %}}
Best practice, hạn chế sử dụng **Root User**.
Thay vào đó hãy tạo IAM User vừa đủ quyền hạn truy cập tài nguyên để dễ dàng quản lý và tránh những rủi ro về bảo mật.
{{% /notice %}}

Ở **section này**,
chúng ta sẽ tạo policy để hạn chế user chỉ có thể tương tác được với **EC2 Instance** trên region **ap-southeast-1**

#### 1. Truy cập AWS IAM Management Console

![iam-management.png](/images/2-restrict-access/iam-management.png)

#### 2. Tạo Custom Policies

2.1. Tại thanh điều hướng bên trái, chọn [Policies](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/policies)

2.2. Tại giao diện **Policies**, chọn **Create policy**

2.3. **Bước 1 - Specify permissions**, chọn **JSON tab** và dán JSON bên dưới vào **Policy Editor** và **Next**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "ap-southeast-1"
        }
      }
    }
  ]
}
```

{{% notice note %}}
Đoạn mã JSON này cho phép tương tác với tài nguyên EC2 nhưng chỉ trong region ap-southeast-1
{{% /notice %}}
2.4. **Bước 2 - Review and create**

- Đặt tên policy tại **Policy name**, **Description**.
- Sau đó chọn **create policy**

  ![review-and-create](/images/2-restrict-access/restricted-policy-1.png)

#### 3. Tạo một User Group và Policy

{{% notice note %}}
Để policy bên trên có thể tái sử dụng, chúng ta có thể gán vào IAM Group. Những IAM User trong mộtGroup đều có quyền hạn như nhau
{{% /notice %}}
3.1. Truy cập [User groups](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/groups) bên thanh điều hướng bên trái

3.2. Trong giao diện **User groups**, chọn **Create group**

3.3. Trong giao diện **Create user group**

- Đặt tên **User group name** và **gán custom policy** vừa tạo.
- Chọn **Create user group**
  ![create-user-group](/images/2-restrict-access/create-user-group.png)
  ![review-user-group](/images/2-restrict-access/review-user-group.png)

#### 4. Tạo User và gán vào User Group

4.1. Truy cập [Users](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/users) bên thanh điều hướng bên trái

4.2. Chọn **Create user**

4.3. Tại **bước 1 - Specify user details**

- Đặt **username**
- Chọn **Provide user access to the AWS Management Console**
- Chọn **I want to create an IAM user**
- Đặt **custom password**
- Chọn **Next**
  ![specify-user-detail](/images/2-restrict-access/specify-user-detail.png)

  4.4. Tại **bước 2 - Set permissions**

- Chọn **Add user to group**
- Chọn group chúng ta vừa tạo e.g. **restricted_ec2_region_group**
- Chọn **Next**
  ![set-permission](/images/2-restrict-access/set-permission.png)

  4.5. Tại **bước 3 - Review and Create**

- Kiểm tra thông tin user và permission
- Chọn **Create user**
  ![review](/images/2-restrict-access/review.png)

  4.6. Tại **bước 4 - Retrieve password**

- Lưu trữ hoặc tải xuống bản **.csv** để quản lý user
  ![review-pwd](/images/2-restrict-access/review-pwd.png)

  4.7. Đăng nhập vào tài khoản IAM User

- Copy **sign-in URL**
- Đăng nhập bằng **username** và **password**
- Thao tác thay đổi **Password**
  ![change-password](/images/2-restrict-access/change-password.png)
- Kiểm tra dịch vụ EC2 trên region **us-east-1** => Policy đã hạn chế quyền sử dụng EC2 tại region này
  ![restricted-region.png](/images/2-restrict-access/restricted-region.png)
- Kiểm tra dịch vụ EC2 trên region **ap-southeast-1**
  ![check_region.png](/images/2-restrict-access/check_region.png)
- Hoàn thành!
