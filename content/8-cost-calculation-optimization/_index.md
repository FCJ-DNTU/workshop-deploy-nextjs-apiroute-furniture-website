---
title: "Cost Calculation & Optimization"
date: 2025
weight: 8
chapter: false
pre: "<b>8. </b>"
---

#### 1. Cost of Each Service

- **Amazon VPC**: VPC does not charge for creating and using basic components such as VPC, subnet, ACL, and security groups. However, using public IP addresses or Elastic IPs may incur costs

- **AWS IAM:** IAM is provided for free. Users can create and manage users, groups, and access permissions without additional costs

- **Amazon EC2** with t2.medium instance:

  - The t2.medium instance provides 2 vCPUs and 4 GB RAM
  - The On-Demand cost for this instance in the ap-southeast-1 region is approximately $0.0464 per hour

- **Amazon DocumentDB** (db.t3.medium instance):

  - The db.t3.medium instance provides 2 vCPUs and 4 GB RAM
  - The On-Demand cost for this instance in the **ap-southeast-1** region is approximately $0.078 per hour
  - Total monthly cost (720 hours): 720 hours x $0.078/hour = ~$56.16
  - Storage: $0.10/GB-month
  - Backup: $0.02/GB-month

- **Amazon CloudFront**:

  - Cost depends on data transfer and the number of requests
  - For example, data transfer out (First 10 TB per month) costs $0.085/GB
  - HTTP/HTTPS requests: $0.0075 per 10,000 requests

Here is an estimated cost table for each AWS service in the **ap-southeast-1** region:

| **Service**    | **Instance Type**           | **Daily Cost ($)**    | **Monthly Cost ($)**   |
| -------------- | --------------------------- | --------------------- | ---------------------- |
| **EC2**        | t2.medium (2 vCPU, 4GB RAM) | 1.11 ($0.0464 x 24h)  | 33.41 ($0.0464 x 720h) |
| **DocumentDB** | db.t3.medium (2 vCPU, 4GB)  | 1.87 ($0.078 x 24h)   | 56.16 ($0.078 x 720h)  |
| **CloudFront** | 100GB Data Transfer         | 8.50 ($0.085 x 100GB) | 255.00 ($0.085 x 3TB)  |
| **VPC**        |                             | Free                  | Free                   |
| **IAM**        |                             | Free                  | Free                   |

#### 2. Cost Optimization

**2.1. Cost Monitoring and Management**

- **Use AWS Cost Explorer**: Monitor real-time costs to control resource usage
- **Set Billing Alerts**: Configure alerts via AWS Budgets to receive notifications when costs exceed a specified threshold
- **Leverage AWS Free Tier**: If new to AWS, take advantage of the free tier services available for the first 12 months

**2.2. Optimizing EC2 Costs**

- **Choose the right instance type**: If the workload is not too high, use t3.medium instead of t2.medium, as t3 offers better performance at a similar price
- **Use Spot Instances**: For non-continuous workloads, Spot Instances can reduce costs by up to 90% compared to On-Demand
- **Implement Auto Scaling**: Set up an Auto Scaling Group to dynamically adjust the number of EC2 instances based on demand, preventing resource wastage
- **Use Reserved Instances / Savings Plans**: If committing to EC2 usage for 1 or 3 years, Reserved Instances or Savings Plans can save up to 72% compared to On-Demand pricing
- **Turn off unused instances**: Shut down EC2 instances when not needed to reduce costs

**2.3. Optimizing Amazon DocumentDB Costs**

- **Reduce the number of replicas**: DocumentDB can have multiple replicas for better read performance, but if unnecessary, keep only **1 primary instance** to save costs
- **Use Auto Scaling Storage:**: DocumentDB automatically scales storage, but regularly monitoring and deleting unnecessary data can prevent extra costs
- **Manage backups wisely**: Amazon DocumentDB provides free backup storage equal to the primary storage size. Additional backups are charged **$0.02/GB-month**, so keep only necessary backups
- **Use Reserved Instances**: If using DocumentDB long-term, Reserved Instances can save up to 60% on costs

**2.4. Optimizing Amazon CloudFront Costs**

- **Enable Compression and Caching**: Use **Gzip/Brotli** compression to reduce data transfer size, saving bandwidth costs
- **Use Regional Edge Caches**: CloudFront caches content closer to users, reducing the load on the origin server and cutting data transfer costs
- **Reduce unnecessary requests**: Optimize static files (CSS, JS, images) and implement Lazy Load to request resources only when needed
- **Leverage CloudFront Free Tier**: AWS provides 1TB of free data transfer per month for the first year. If traffic is low, use Free Tier benefits to lower costs

**2.5. Optimizing VPC and IAM Costs**

- **Minimize Elastic IP usage**: Elastic IPs are free when attached to a running instance. If not in use, release them to avoid a **$0.005/hour** charge
- **Remove unused resources**: Regularly check and delete **subnets, security groups, NAT Gateways, and VPN Gateways** that are no longer needed to avoid unnecessary costs
- **Manage IAM access**: Limit the creation of IAM Users and Roles, granting access only when necessary to reduce security risks
