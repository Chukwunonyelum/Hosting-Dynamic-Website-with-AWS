
# AWS 3-Tier Web Application Deployment

This repository provides a comprehensive guide for deploying a dynamic web application on AWS using a 3-tier architecture. The solution leverages AWS services such as EC2, S3, RDS, and an Application Load Balancer to address the challenge of scalability, high availability, and secure data management for dynamic web applications.

## Business Problem

### Problem Overview:
Businesses that manage dynamic web applications often face several key challenges:
1. **Scalability**: As the application grows, it needs to handle an increasing number of users without compromising performance.
2. **High Availability**: Ensuring the web application is always accessible and has minimal downtime.
3. **Data Security and Management**: Securely managing data in databases and ensuring the integrity of sensitive information.
4. **Cost Management**: Optimizing the cost of infrastructure, while ensuring that the system can handle real-time traffic spikes and database loads.

### Proposed Solution:
The solution implements a **3-tier architecture** that separates the web (presentation), application (logic), and database (storage) layers. By leveraging AWS services, we ensure:
- **Scalability** through EC2 Auto Scaling and Application Load Balancers.
- **High Availability** using multiple availability zones (AZs) and load balancing.
- **Data Security** with private subnets, secure RDS instances, and proper IAM role management.
- **Cost Optimization** by scaling the infrastructure dynamically based on demand.

---

## Solution Architecture

The 3-tier architecture is broken down into three core layers:
1. **Web Tier**: This layer handles user requests and is deployed on EC2 instances behind an Application Load Balancer.
2. **Application Tier**: The logic and business rules are processed in this layer using a separate set of EC2 instances.
3. **Database Tier**: Data is managed in an RDS (Relational Database Service) instance, which is kept in a private subnet for security.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [1. S3 Bucket Setup](#1-s3-bucket-setup)
- [2. VPC and Networking Setup](#2-vpc-and-networking-setup)
- [3. EC2 Instances Setup](#3-ec2-instances-setup)
- [4. RDS and Data Migration](#4-rds-and-data-migration)
- [5. Application Load Balancer](#5-application-load-balancer)
- [6. Website Installation](#6-website-installation)
- [7. DNS and SSL Setup](#7-dns-and-ssl-setup)
- [8. Auto Scaling Group](#8-auto-scaling-group)
- [Business Problem and Solution](#business-problem)

---

## Prerequisites

1. AWS account
2. AWS CLI installed and configured
3. A domain name (for DNS setup)
4. Web application code ready for deployment

---

## 1. S3 Bucket Setup

1. Create an S3 bucket in AWS.
2. Upload your web application files (static files, SQL scripts, etc.) to the bucket.
3. Optionally configure the bucket for public access if required.

---

## 2. VPC and Networking Setup

### 2.1. Create a 3-Tier VPC
- Navigate to the **VPC** dashboard in AWS.
- Create a VPC with public, private, and database subnets.

### 2.2. Enable DNS Hostname
- Ensure DNS resolution and DNS hostname options are enabled for the VPC.

### 2.3. Internet Gateway Setup
- Create and attach an **Internet Gateway** to the VPC for outbound traffic.

### 2.4. Subnet Creation
- Create **public** and **private subnets** for each tier of the architecture.

### 2.5. Public Subnet: Auto-assign Public IPs
- Enable **auto-assign public IP** for public subnets to allow external access to EC2 instances.

### 2.6. NAT Gateway
- Create a **NAT Gateway** for instances in private subnets to access the internet.

### 2.7. Routing Tables
- Create separate **route tables** for public and private subnets.
- Associate the public route table with the **Internet Gateway** and the private route table with the **NAT Gateway**.

---

## 3. EC2 Instances Setup

### 3.1. Create EC2 Instance Connect Endpoint
- Create an **EC2 Instance Connect endpoint** in your VPC.

### 3.2. Launch EC2 Instance
- Launch an EC2 instance in the **public subnet** for the web tier.

### 3.3. SSH into EC2 Instance
- Connect to the instance using the AWS Management Console or AWS CLI.

### 3.4. Terminate EC2 Instance
- Once the instance is no longer needed, terminate it to save costs.

---

## 4. RDS and Data Migration

### 4.1. Create RDS Instance
- Go to the **RDS** console.
- Create an RDS instance for the application database in the **private subnet**.

### 4.2. Upload SQL Scripts to S3
- Upload SQL migration scripts to the previously created S3 bucket.

### 4.3. IAM Role for S3 Access
- Create an **IAM role** with access to S3 and attach it to an EC2 instance for data migration.

### 4.4. Data Migration with Flyway
- Use **Flyway** or a similar tool to migrate your SQL scripts from S3 to the RDS instance.

### 4.5. Terminate EC2 Instance
- Once data migration is complete, terminate the EC2 instance used for migration.

---

## 5. Application Load Balancer

### 5.1. Create Target Group
- Create a **target group** for your EC2 instances.

### 5.2. Launch EC2 Instance
- Launch another EC2 instance for the web tier and add it to the target group.

### 5.3. Create Application Load Balancer
- Set up an **Application Load Balancer (ALB)** to distribute traffic across the EC2 instances.

---

## 6. Website Installation

### 6.1. Prepare Installation Script
- Write a script to automate the installation of your web application on EC2 instances.

### 6.2. Install Website
- Run the script to install the website on the EC2 instance in the web tier.

---

## 7. DNS and SSL Setup

### 7.1. Register Domain in Route 53
- Use **Route 53** to register a domain or configure DNS settings.

### 7.2. Create Record Set
- Create an **A record** or **CNAME** that points to the **Application Load Balancer**.

### 7.3. Request SSL Certificate
- Use **AWS Certificate Manager (ACM)** to request an SSL certificate for the domain.

### 7.4. HTTPS Listener
- Add an HTTPS listener to the **Application Load Balancer** and attach the SSL certificate for secure traffic handling.

---

## 8. Auto Scaling Group

### 8.1. Create AMI
- Once the EC2 instance is configured, create an **Amazon Machine Image (AMI)** for future use.

### 8.2. Terminate EC2 Instance
- Terminate the EC2 instance after creating the AMI to save costs.

### 8.3. Launch Template
- Create a **Launch Template** using the AMI.

### 8.4. Create Auto Scaling Group
- Set up an **Auto Scaling Group** to manage the number of EC2 instances behind the Application Load Balancer.

---

## Conclusion

This solution addresses key business challenges like scalability, high availability, and secure data management by deploying a 3-tier architecture on AWS. The web app is deployed using EC2 instances, an RDS database, and an Application Load Balancer with Auto Scaling. This architecture optimizes performance and cost while ensuring reliability.

Feel free to contribute to this repository or open issues if you have any questions or suggestions.

