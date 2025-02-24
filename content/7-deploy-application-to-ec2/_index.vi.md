---
title: "Triển khai ứng dụng lên EC2 instance"
date: 2025
weight: 7
chapter: false
pre: "<b>7. </b>"
---

Chúng ta đã hoàn thành xây dựng Infrastructure, tiếp theo chỉ cần triển khai ứng dụng Go lên EC2 Instance là hoàn thành
bài lab!!! xd

#### Cấu trúc dự án

```
workshop-01
│───assets              # Chứa hình ảnh sử dụng trong dự án
│
└───static              # Chứa các file tĩnh
│    └───css            # Lưu trữ các file CSS dùng để thiết kế
│         │   styles.css
│
└───templates            # Lưu trữ các template HTML
│   │   article.html     # Template hiển thị bài viết
│   │   base.html        # Template cơ bản, được dùng chung cho các template khác
│   │   edit.html        # Template chỉnh sửa bài viết
│   │   index.html       # Template trang chủ
│   │   new.html         # Template tạo bài viết mới
│
│   .env.example         # File mẫu chứa biến môi trường
│   .gitignore           # Quy định các file và thư mục sẽ bị bỏ qua khi sử dụng Git
│   db.go                # Chứa logic tương tác với cơ sở dữ liệu
│   go.mod               # Định nghĩa version của Go module và các thư viện phụ thuộc
│   go.sum               # Chứa checksums của các thư viện phụ thuộc
│   main.go              # Logic chính của ứng dụng, bao gồm render và thực hiện các thao tác CRUD cho bài viết
│   README.md            # Tài liệu mô tả về dự án

```

#### 1. Cài đặt Git version control và Golang

- Cài đặt git

```
$ sudo yum install git -y
$ git --version
```

- Cài đặt golang

```
  $ wget https://go.dev/dl/go1.23.4.linux-amd64.tar.gz
  $ sudo tar -C /usr/local -xzf go1.23.4.linux-amd64.tar.gz
  $ export PATH=$PATH:/usr/local/go/bin
  $ go version
```

![installation.png](/images/7-deploy-app-to-ec2/installation.png)

#### 2. Clone repository

- Clone repository Blog application của mình, hoặc các bạn có thể sử dụng repository cá nhân.
  `$ git clone https://github.com/minhnghia2k3/workshop-01.git`

#### 3. Export environment variables

Thêm biến môi trường, các biến môi trường này là phần required trong application

```
$ vi ~/.bashrc
export DATABASE_URL="admin:Admin123@tcp(<YOUR_DB_ENDPOINT>:3306)/blog_db"
export AWS_REGION="ap-southeast-1"
export S3_BUCKET_NAME="minhnghia2k3-blog-s3-bucket"

$ source ~/.bashrc
```

![export_env.png](/images/7-deploy-app-to-ec2/export_env.png)

#### 4. Khởi chạy ứng dụng

```
$ ls
$ cd workshop-01
$ go build -o ./bin/main .
$ ./bin/main
```

- Thành quả chạy thành công, server sẽ open tại port **:3000**

![run_app.png](/images/7-deploy-app-to-ec2/run_app.png)

#### 5. Kiểm tra Deployment và chức năng

- Truy cập EC2 domain e.g. http://ec2-13-250-114-245.ap-southeast-1.compute.amazonaws.com:3000/
  ![website.png](/images/7-deploy-app-to-ec2/website.png)
- Test chức năng tạo blog
  ![create-blog.png](/images/7-deploy-app-to-ec2/create-blog.png)
- Test chức năng chỉnh sửa blog
  ![edit-blog.png](/images/7-deploy-app-to-ec2/edit-blog.png)
- Test chức năng xóa blog

---

![finish.png](/images/7-deploy-app-to-ec2/finish.png)

#### 6. Kiểm tra lưu trữ tại Mysql và S3 bucket

Tại SSH của EC2 Instance:

**1. Kiểm tra Mysql**

```
$ mysql -h mysql-golang-db.c1a20mqwgeb9.ap-southeast-1.rds.amazonaws.com -P 3306 -u admin -pAdmin123
$ USE blog_db;
$ SELECT * FROM articles;
```

| id  | title                                      | content                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| --- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | Introduction to Golang: A Beginner's Guide | <p>Golang, or Go, is an open-source programming language designed by Google. Known for its simplicity, concurrency support, and performance, Go is an excellent choice for building scalable web applications, cloud-native solutions, and microservices.<br><span style="background-color: rgb(194, 224, 244);">`fmt.Println("Hello Go!")`</span></p><p><span style="background-color: rgb(194, 224, 244);"><img src="https://minhnghia2k3-blog-s3-bucket.s3.amazonaws.com/uploads/1736500595218035330.png"></span></p> |

**2. Kiểm tra Bucket**

- Tại folder **uploads/**, có thể thấy chúng ta đã lưu trữ thành công từ application về S3 bucket
  ![check_bucket.png](/images/7-deploy-app-to-ec2/check-bucket.png)
