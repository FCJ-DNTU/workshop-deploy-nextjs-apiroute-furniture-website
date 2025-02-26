---
title: "Hạn chế quyền truy cập với IAM Service"
date: 2025
weight: 2
chapter: false
pre: "<b>2. </b>"
---

{{% notice note %}}
**Best practice**, hạn chế sử dụng **Root User**.
Thay vào đó hãy tạo IAM User vừa đủ quyền hạn truy cập tài nguyên để dễ dàng quản lý và tránh những rủi ro về bảo mật.
{{% /notice %}}

Ở **section này**,
chúng ta sẽ tạo policy để hạn chế user chỉ có thể tương tác được với **EC2 Instance** trên region **ap-southeast-1**

#### 1. Truy cập AWS IAM Management Console

![iam-management.png](/images/2-restrict-access/2.1.png)

#### 2. Tạo Custom Policies

2.1. Tại thanh điều hướng bên trái, chọn
[Policies](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/policies)
![policies.png](/images/2-restrict-access/2.2.png)

2.2. Tại giao diện **Policies**, chọn **Create policy**
![create-policy.png](/images/2-restrict-access/2.3.png)

2.3.

**Bước 1 - Trong Specify permissions**, chọn **JSON tab** và dán JSON bên dưới vào **Policy Editor** và **Next**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
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

![Specify permissions.png](/images/2-restrict-access/2.4.png)

{{% notice note %}}
Cho phép user sử dụng các thao tác của EC2 chỉ trong region ap-southeast-1
{{% /notice %}}
2.4.

**Bước 2 - Review and create**

- Đặt tên policy tại **Policy name** `ec2-restricted-region`, **Description** `Allow access EC2 from region Singapore - ap-southeast-1`.
- Sau đó chọn **Create policy**

![review-and-create](/images/2-restrict-access/2.5.png)

#### 3. Tạo một User Group và Policy

{{% notice note %}}
Để policy bên trên có thể tái sử dụng, chúng ta có thể gán vào IAM Group. Những IAM User trong một Group đều có quyền hạn
như nhau
{{% /notice %}}
3.1. Truy cập [User groups](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/groups) bên thanh
điều hướng bên trái
![user-group](/images/2-restrict-access/2.6.png)

3.2. Trong giao diện **User groups**, chọn **Create group**

3.3. Trong giao diện **Create user group**

- Đặt tên **User group name** `ec2-restricted-group`, ở phần **Filter by Type** chọn **Customer Managed** và chọn custom policy vừa mới tạo
- Chọn **Create user group**
  ![create-user-group](/images/2-restrict-access/2.7.png)

- Sau khi tạo xong User Group sẽ hiển thị giao diện như này
  ![review-user-group](/images/2-restrict-access/2.8.png)

#### 4. Tạo User và gán vào User Group

4.1. Truy cập [Users](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/users) bên thanh điều
hướng bên trái
![user-tab](/images/2-restrict-access/2.9.png)

4.2. Chọn **Create user**

4.3.

Tại **Bước 1 - Specify user details**

- Đặt **username** `restricted_user`
- Chọn **Provide user access to the AWS Management Console**
- Chọn **I want to create an IAM user**
- Đặt **Custom password** `restricted_user1`
- Chọn **Next**
  ![specify-user-detail](/images/2-restrict-access/2.10.png)

  4.4.

  Tại **Bước 2 - Set permissions**

- Chọn **Add user to group**
- Chọn group chúng ta vừa tạo e.g. **ec2-restricted-group**
- Chọn **Next**
  ![set-permission](/images/2-restrict-access/2.11.png)

  4.5.

  Tại **Bước 3 - Review and Create**

- Kiểm tra thông tin **user** và **permission**
- Chọn **Create user**
  ![review](/images/2-restrict-access/2.12.png)

  4.6.

  Tại **Bước 4 - Retrieve password**

- Lưu trữ hoặc tải xuống bản **.csv** để quản lý user
  ![review-pwd](/images/2-restrict-access/2.13.png)

  4.7. Đăng nhập vào tài khoản IAM User

- Copy **sign-in URL**
- Đăng nhập bằng **username** `restricted_user` và **password** `restricted_user1`
  ![sign-in](/images/2-restrict-access/2.14.png)
- Sau khi **Sign in** thì thực hiện thao tác thay đổi mật khẩu
  ![change-password](/images/2-restrict-access/2.15.png)
- Kiểm tra dịch vụ EC2 trên region **us-east-2** => Policy đã hạn chế quyền sử dụng EC2 tại region này
  ![restricted-region.png](/images/2-restrict-access/2.16.png)
- Kiểm tra dịch vụ EC2 trên region **ap-southeast-1**
  ![check_region.png](/images/2-restrict-access/2.17.png)

{{< center >}}

### **Hoàn thành! 🚀**

{{< /center >}}
