---
title: "Restrict Access with IAM Service"
date: 2025
weight: 2
chapter: false
pre: "<b>2. </b>"
---

{{% notice note %}}
**Best practice**: Avoid using the **Root User**. Instead, create IAM Users with minimal permissions needed to manage
resources. This makes management easier and reduces security risks.
{{% /notice %}}

In this section, we will create a policy to restrict a user to only interact with **EC2 Instances** in the
**ap-southeast-1** region.

#### 1. Access the AWS IAM Management Console

![iam-management.png](/images/2-restrict-access/2.1.png)

#### 2. Create Custom Policies

2.1. On the left navigation menu, select
[Policies](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/policies).
![policies.png](/images/2-restrict-access/2.2.png)

2.2. In the **Policies** interface, click **Create policy**.
![create-policy.png](/images/2-restrict-access/2.3.png)

2.3.

**Step 1 - Specify permissions**:

- Go to the **JSON tab**, paste the following JSON into the **Policy Editor**, and click **Next**.

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
This policy allows users to manage EC2 instances only in the ap-southeast-1 region.
{{% /notice %}}

2.4.

**Step 2 - Review and Create**:

- Enter a **Policy Name**: `ec2-restricted-region`.
- Enter a **Description**: `Allow access to EC2 only in ap-southeast-1`.
- Click **Create policy**.

![review-and-create](/images/2-restrict-access/2.5.png)

#### 3. Create a User Group and Assign the Policy

{{% notice note %}}
To reuse this policy, assign it to an IAM Group. All IAM Users in this group will have the same permissions.
{{% /notice %}}

3.1. Access [User groups](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/groups) from the left
navigation menu.
![user-group](/images/2-restrict-access/2.6.png)

3.2. In the **User groups** interface, click **Create group**.

3.3. In the **Create user group** interface:

- Enter **User Group Name**: `ec2-restricted-group`.
- Under **Filter by Type**, select **Customer Managed**.
- Choose the policy created earlier.
- Click **Create user group**.
  ![create-user-group](/images/2-restrict-access/2.7.png)
- After creation, you should see a summary like this:
  ![review-user-group](/images/2-restrict-access/2.8.png)

#### 4. Create a User and Assign Them to the Group

4.1. Access [Users](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/users) from the left
navigation menu.
![user-tab](/images/2-restrict-access/2.9.png)

4.2. Click **Create user**.

4.3.

**Step 1 - Specify user details**:

- Enter a **username**: `restricted_user`.
- Select **Provide user access to the AWS Management Console**.
- Choose **I want to create an IAM user**.
- Set a **custom password**: `restricted_user1`.
- Click **Next**.
  ![specify-user-detail](/images/2-restrict-access/2.10.png)

  4.4.

**Step 2 - Set permissions**:

- Select **Add user to group**.
- Choose the group created earlier (e.g., **ec2-restricted-group**).
- Click **Next**.
  ![set-permission](/images/2-restrict-access/2.11.png)

  4.5.

**Step 3 - Review and Create**:

- Review the **user** and **permissions**.
- Click **Create user**.
  ![review](/images/2-restrict-access/2.12.png)

  4.6.

**Step 4 - Retrieve password**:

- Save or download the **.csv** file to manage user credentials.
  ![review-pwd](/images/2-restrict-access/2.13.png)

  4.7. Log in as the IAM User:

- Copy the **sign-in URL**.
- Log in using the **username**: `restricted_user` and **password**: `restricted_user1`.
  ![sign-in](/images/2-restrict-access/2.14.png)
- After signing in, change your password.
  ![change-password](/images/2-restrict-access/2.15.png)

#### Testing:

- Check the EC2 service in **us-east-2**: The policy should restrict access in this region.
  ![restricted-region.png](/images/2-restrict-access/2.16.png)

- Check the EC2 service in **ap-southeast-1**: Access should be allowed.
  ![check_region.png](/images/2-restrict-access/2.17.png)

{{< center>}}

### **Completed! 🚀**

{{< /center>}}
