---
title: "Create CloudFront"
date: 2025
weight: 7
chapter: false
pre: "<b>7. </b>"
---

We have completed setting up the infrastructure. Now, we just need to deploy the Go application to the EC2 instance to
complete the lab! xd

#### Project Structure

```
workshop-01
│───assets # Contains images used in the project
│
└───static # Contains static files
│ └───css # Stores CSS files for styling
│ │ styles.css
│
└───templates # Stores HTML templates
│ │ article.html # Template for displaying an article
│ │ base.html # Base template used across other templates
│ │ edit.html # Template for editing an article
│ │ index.html # Template for the homepage
│ │ new.html # Template for creating a new article
│
│ .env.example # Example environment variables file
│ .gitignore # Git ignore rules for the project
│ db.go # Contains logic for database interactions
│ go.mod # Specifies Go module version and dependencies
│ go.sum # Contains checksums for dependencies
│ main.go # Main application logic, includes rendering and CRUD operations for articles
│ README.md # Documentation for the project
```

#### 1. Install Git version control and Golang

- Install Git:

```
$ sudo yum install git -y
$ git --version
```

- Install Golang:

```
$ wget https://go.dev/dl/go1.23.4.linux-amd64.tar.gz
$ sudo tar -C /usr/local -xzf go1.23.4.linux-amd64.tar.gz
$ export PATH=$PATH:/usr/local/go/bin
$ go version
```

![installation.png](/images/7-deploy-app-to-ec2/installation.png)

#### 2. Clone the repository

- Clone the Blog application repository, or you can use your personal repository.

```
$ git clone https://github.com/minhnghia2k3/workshop-01.git
```

#### 3. Export environment variables

Add the required environment variables for the application:

```
$ vi ~/.bashrc
export DATABASE_URL="admin:Admin123@tcp(<YOUR_DB_ENDPOINT>:3306)/blog_db"
  export AWS_REGION="ap-southeast-1"
  export S3_BUCKET_NAME="minhnghia2k3-blog-s3-bucket"

  $ source ~/.bashrc
  ```

  ![export_env.png](/images/7-deploy-app-to-ec2/export_env.png)

  #### 4. Run the application

  ```
  $ ls
  $ cd workshop-01
  $ go build -o ./bin/main .
  $ ./bin/main
  ```

  - If successful, the server will open on port **:3000**.

  ![run_app.png](/images/7-deploy-app-to-ec2/run_app.png)

  #### 5. Test Deployment and Functionality

  - Access the EC2 domain, e.g., http://ec2-13-250-114-245.ap-southeast-1.compute.amazonaws.com:3000/
  ![website.png](/images/7-deploy-app-to-ec2/website.png)
  - Test blog creation:
  ![create-blog.png](/images/7-deploy-app-to-ec2/create-blog.png)
  - Test blog editing:
  ![edit-blog.png](/images/7-deploy-app-to-ec2/edit-blog.png)
  - Test blog deletion.

  ---

  ![finish.png](/images/7-deploy-app-to-ec2/finish.png)

  #### 6. Check Storage in MySQL and S3 Bucket

  From the EC2 SSH terminal:

  **1. Check MySQL:**

  ```
  $ mysql -h mysql-golang-db.c1a20mqwgeb9.ap-southeast-1.rds.amazonaws.com -P 3306 -u admin -pAdmin123
  $ USE blog_db;
  $ SELECT * FROM articles;
  ```

  | id | title | content |
  | --- | ------------------------------------------ |
  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  |
  | 1 | Introduction to Golang: A Beginner's Guide | <p>Golang, or Go, is an open-source programming language designed
    by Google. Known for its simplicity, concurrency support, and performance, Go is an excellent choice for building
    scalable web applications, cloud-native solutions, and microservices.<br><span
      style="background-color: rgb(194, 224, 244);">`fmt.Println("Hello Go!")`</span></p>
  <p><span style="background-color: rgb(194, 224, 244);"><img
        src="https://minhnghia2k3-blog-s3-bucket.s3.amazonaws.com/uploads/1736500595218035330.png"></span></p> |

  **2. Check the Bucket:**

  - In the **uploads/** folder, you can see that the application successfully stored the file to the S3 bucket.
  ![check_bucket.png](/images/7-deploy-app-to-ec2/check-bucket.png)