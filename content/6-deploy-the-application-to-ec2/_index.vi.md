---
title: "Triển khai ứng dụng lên EC2"
date: 2025
weight: 6
chapter: false
pre: "<b>6. </b>"
---

#### 1. Cài đặt Git và NodeJS

- Cài đặt Git

```shell
$ sudo apt install -y git
$ git --version
```

- Cài đặt NodeJS và npm

```shell
$ curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
$ sudo apt install -y nodejs
$ node -v
$ npm -v
```

- Cài đặt PM2 để quản lý ứng dụng

```shell
$ sudo npm install -g pm2
$ pm2 -v
```

#### 2. Clone Repository

```shell
$ git clone https://github.com/nhungnguyen-9/e-commerce-furniture.git
```

- Dùng lệnh **`ls`** để kiểm tra xem project đã được clone về chưa
![clone-project](/images/6-deploy-the-application-to-ec2/6.1.png)

- Vào thư mục **e-commerce-furniture**. Sau đó, cài đặt các dependencies

```shell
$ cd e-commerce-furniture
$ npm install
```

#### 3. Migrate data từ MongoDB đến Amazon DocumentDB

- Trong workshop này, chúng ta sẽ sử dụng phương pháp migrate **offline**. Bạn có thể tham khảo các cách làm khác ở bài
blog này: [Migrate from MongoDB to Amazon
DocumentDB](https://aws.amazon.com/blogs/database/migrate-from-mongodb-to-amazon-documentdb-using-the-offline-method/)
![migrate-offline](/images/6-deploy-the-application-to-ec2/offline-migration-approach.gif)

- Cài đặt **mongosh**

```shell
$ wget -qO - https://pgp.mongodb.com/server-6.0.asc | sudo tee /usr/share/keyrings/mongodb-server-key.asc
$ echo "deb [signed-by=/usr/share/keyrings/mongodb-server-key.asc] https://repo.mongodb.org/apt/ubuntu
focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
$ sudo apt update
$ sudo apt install -y mongodb-mongosh
$ mongosh --version
```

- Kiểm tra kết nối với **DocumentDB**

- Vào giao diện **Cluster** của **Amazon DocumentDB**, sau đó chọn cluster đã tạo
![choose-cluster](/images/6-deploy-the-application-to-ec2/6.3.png)
- Tải file **global-bundle.pem** về EC2 và kết nối tới DocumentDB trong EC2 (thay thế **insertYourPassword** trong chuỗi
kết nối thành password bạn đã đặt cho DocumentDB)
![connect](/images/6-deploy-the-application-to-ec2/6.4.png)
![connect2](/images/6-deploy-the-application-to-ec2/6.5.png)
- Gõ lệnh **show dbs**, bạn sẽ thấy hiện tại trong DocumentDB chưa có database nào hết
![check](/images/6-deploy-the-application-to-ec2/6.6.png)

- Tiếp theo, chúng ta sẽ tiến hành migrate data

- Cài đặt **MongoDB Database Tools**

```shell
$ wget https://fastdl.mongodb.org/tools/db/mongodb-database-tools-ubuntu2204-x86_64-100.9.0.tgz
$ tar -xvzf mongodb-database-tools-ubuntu2204-x86_64-100.9.0.tgz
$ sudo mv mongodb-database-tools-ubuntu2204-x86_64-100.9.0/bin/* /usr/local/bin/
$ sudo apt install -y mongodb-mongosh
$ mongodump --version
```

- **Dump** dữ liệu từ MongoDB, dữ liệu sẽ được sao lưu từ MongoDB vào một thư mục tên **backup**

```shell
$ mongodump
--uri="mongodb+srv://nguyennhung9846:3siHCKWL69z5DoS1@nhaxinh.02uuoha.mongodb.net/?retryWrites=true&w=majority&appName=nhaxinh"
--out=backup
$ ls
```

![mongodump](/images/6-deploy-the-application-to-ec2/6.7.png)

- **Restore** dữ liệu lên DocumentDB
- Vào giao diện của Cluster đã tạo, sau đó copy chuỗi kết nối sau
![mongorestore](/images/6-deploy-the-application-to-ec2/6.8.png)
- Gõ lệnh sau để phục hồi dữ liệu lên DocumentDB (thay thế **insertYourPassword** trong chuỗi kết nối thành password bạn
đã đặt cho DocumentDB)

```shell
$ mongorestore --uri="mongodb://user123:<insertYourPassword>
  @docdb-nextjs-workshop.cluster-c10k88ou8amc.ap-southeast-1.docdb.amazonaws.com:27017/?tls=true&tlsCAFile=global-bundle.pem&replicaSet=rs0&readPreference=secondaryPreferred&retryWrites=false"
  --dir=backup --drop
  $ ls
  ```

  ![mongorestore](/images/6-deploy-the-application-to-ec2/6.9.png)

  - Sau khi restore xong, kết nối vào lại DocumentDB
  - Trong giao diện của **Cluster docdb-nextjs-workshop**, copy **Connect to this cluster with the mongo shell** rồi sau
  đó dán vào EC2 để kiểm tra kết nối
  ![check-connect](/images/6-deploy-the-application-to-ec2/6.10.png)
  ![check-connect](/images/6-deploy-the-application-to-ec2/6.11.png)
  - Sau đó kiểm tra database và collections

  ```shell
  $ show dbs
  $ use test
  $ show collections
  ```

  ![check-db](/images/6-deploy-the-application-to-ec2/6.12.png)

  #### 4. Thêm biến môi trường (.env)

  - Tạo file **.env** chứa thông tin kết nối database và cấu hình ứng dụng

  ```shell
  $ touch .env
  $ nano .env
  ```

  - Thêm các biến môi trường
  Tải file **.env** và thêm các biến môi trường vào
  **[.env](https://drive.google.com/file/d/1PH2-dZjuWKzp2cHs6MVGqb57LLpHhWoR/view?usp=sharing)**

  ![env](/images/6-deploy-the-application-to-ec2/6.13.png)

  #### 5. Build và chạy ứng dụng

  - Gõ lệnh **build**

  ```
  npm run build
  ```

  - Chạy ứng dụng với **pm2**

  ```
  pm2 start npm --name "nextjs-app" -- run start
  pm2 save
  pm2 startup
  ```

  - Kiểm tra ứng dụng có chạy không:

  ```
  pm2 list
  ```

  ![build](/images/6-deploy-the-application-to-ec2/6.14.png)

  #### 6. Kiểm tra deployment

  - Mở trình duyệt và truy cập

  ```
  http://your-ec2-public-ip:3000
  ```

  ![deploy](/images/6-deploy-the-application-to-ec2/6.15.png)

  #### 7. Kiểm tra lưu trữ tại DocumentDB

  - Vào Cluster đã tạo, copy chuỗi kết nối trong phần **Connect to this cluster with an application**, rồi vào EC2, gõ
  lệnh (thay thế **insertYourPassword** thành password bạn đã đặt cho DocumentDB)

  ```
  mongosh docdb-nextjs-workshop.cluster-c10k88ou8amc.ap-southeast-1.docdb.amazonaws.com:27017 --tls --tlsCAFile
  global-bundle.pem --retryWrites=false --username user123 --password user1234
  ```

  - Kiểm tra dữ liệu

  ```
  use test
  show collections
  db.your-collection.find().pretty()
  ```

  ![check-database](/images/6-deploy-the-application-to-ec2/6.16.png)

  {{< center>}}

    ### **Hoàn thành! 🚀**

    {{< /center>}}