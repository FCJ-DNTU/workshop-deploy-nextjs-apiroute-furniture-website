---
title: "Restrict Access with IAM Service"
date: 2025
weight: 2
chapter: false
pre: "<b>2. </b>"
---

{{% notice note %}}
Best practice: Avoid using the **Root User**. Instead, create IAM Users with minimal permissions needed to manage resources. This makes management easier and reduces security risks.
{{% /notice %}}

In this section, we will create a policy to restrict a user to only interact with **EC2 Instances** in the **ap-southeast-1** region.

#### 1. Access the AWS IAM Management Console

![iam-management.png](/images/2-restrict-access/iam-management.png)

#### 2. Create Custom Policies

2.1. On the left navigation menu, select [Policies](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/policies).

2.2. In the **Policies** interface, click **Create policy**.

2.3. **Step 1 - Specify permissions**:

- Go to the **JSON tab**, paste the following JSON into the **Policy Editor**, and click **Next**.

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
This JSON allows interactions with EC2 resources but only in the **ap-southeast-1** region.
{{% /notice %}}

2.4. **Step 2 - Review and Create**:

- Provide a name and description for the policy.
- Click **Create policy**.

  ![review-and-create](/images/2-restrict-access/restricted-policy-1.png)

#### 3. Create a User Group and Assign the Policy

{{% notice note %}}
To reuse the above policy, assign it to an IAM Group. All IAM Users in the group will share the same permissions.
{{% /notice %}}

3.1. Access [User groups](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/groups) from the left navigation menu.

3.2. In the **User groups** interface, click **Create group**.

3.3. In the **Create user group** interface:

- Name the group and assign the custom policy created earlier.
- Click **Create user group**.  
  ![create-user-group](/images/2-restrict-access/create-user-group.png)  
  ![review-user-group](/images/2-restrict-access/review-user-group.png)

#### 4. Create a User and Assign Them to the Group

4.1. Access [Users](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/users) from the left navigation menu.

4.2. Click **Create user**.

4.3. **Step 1 - Specify user details**:

- Enter a **username**.
- Select **Provide user access to the AWS Management Console**.
- Choose **I want to create an IAM user**.
- Set a **custom password**.
- Click **Next**.  
   ![specify-user-detail](/images/2-restrict-access/specify-user-detail.png)

  4.4. **Step 2 - Set permissions**:

- Select **Add user to group**.
- Choose the group created earlier (e.g., **restricted_ec2_region_group**).
- Click **Next**.  
   ![set-permission](/images/2-restrict-access/set-permission.png)

  4.5. **Step 3 - Review and Create**:

- Review the user and permissions.
- Click **Create user**.  
   ![review](/images/2-restrict-access/review.png)

  4.6. **Step 4 - Retrieve password**:

- Save or download the **.csv** file to manage user credentials.  
   ![review-pwd](/images/2-restrict-access/review-pwd.png)

  4.7. Log in as the IAM User:

- Copy the **sign-in URL**.
- Log in using the **username** and **password**.
- Update the **password** when prompted.  
  ![change-password](/images/2-restrict-access/change-password.png)

#### Testing:

- Check the EC2 service in **us-east-1**: The policy should restrict access in this region.  
  ![restricted-region.png](/images/2-restrict-access/restricted-region.png)

- Check the EC2 service in **ap-southeast-1**: Access should be allowed.  
  ![check_region.png](/images/2-restrict-access/check_region.png)

# **Done!**
