---
title: "Create S3 Instance"
date: 2025
weight: 6
chapter: false
pre: "<b>6. </b>"
---

To store images for our application, we will use AWS S3 storage service.

#### Simple Storage Service (Amazon S3)

**Amazon Simple Storage Service (Amazon S3)** is an object storage service that offers industry-leading scalability, data availability, security, and performance.

#### Set up S3 Storage Service

1. Visit [AWS S3](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1#)

2. Select **Create bucket**:

   - Choose **AWS Region**: **Asia Pacific (Singapore) ap-southeast-1**
   - Enter a unique name for your **S3 bucket**.
   - Since we want the images to be publicly accessible for the client, **uncheck** **Block all public access**, and check **Confirm**.
     ![create-bucket.png](/images/6-create-s3-instance/create-bucket.png)
   - Click **Create bucket**.

3. Access the S3 Bucket:

   - Once the bucket is created, go to your S3 bucket and check its details.
     ![s3-bucket.png](/images/6-create-s3-instance/s3-bucket.png)

4. Upload an object to the S3 Bucket:

   - Go to your S3 bucket, select **Upload**.
   - Drag and drop a file into the **Upload** box and click **Upload**.
     ![upload-object.png](/images/6-create-s3-instance/upload-object.png)

5. Create an IAM Role for EC2:
   {{% notice note %}}
   Best practices: Instead of connecting to S3 using **Access Key, Secret Key**, for enhanced security, use an **IAM Role** to authorize an **EC2 Instance** to connect to the **S3 Bucket**.
   {{% /notice %}}

- First, create a **role** for the **EC2 Service** to access the **S3 bucket**:
- Go to [Roles](https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/roles), click **Create Role**.
- In the **Select trusted entity** interface, choose **AWS Service** as the entity type.
- Select **EC2** as the use case, and click **Next**.
  ![iam-role.png](/images/6-create-s3-instance/iam-role.png)
- Then, choose **AmazonS3FullAccess**, and click **Next** (For more security, you can create a **Custom Policy**).
  ![policy.png](/images/6-create-s3-instance/policy.png)
- After that, set the **Role name**, **Description**, and review the **Role Details**.

6. Attach the Role to EC2:

- Go to [EC2 Instances](https://ap-southeast-1.console.aws.amazon.com/ec2/home?region=ap-southeast-1#Instances:).
- Select the EC2 Instance you created.
- Choose **Actions** > **Security** > **Modify IAM Role**.
  ![attach-role.png](/images/6-create-s3-instance/attach-role.png)
- Select the IAM Role you just created and click **Update IAM Role**.
  ![update-iam-role.png](/images/6-create-s3-instance/update-iam-role.png)

7. Test the connection between EC2 and S3:

- Access the EC2 instance and run `$ aws s3 ls s3://my-bucket-name`.

![ec2-to-s3.png](/images/6-create-s3-instance/ec2-to-s3.png)
