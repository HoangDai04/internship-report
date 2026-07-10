---

title: "Project Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
--------------------

# GYM WEBSITE SYSTEM ARCHITECTURE & DEPLOYMENT

### 1. Executive Summary

This project focuses on designing and deploying a modern full-stack web platform for gym management and enhancing the overall member experience. The system is built on Amazon Web Services (AWS), combining scalable cloud infrastructure with managed AWS services. The primary objectives are to ensure high availability, maintain strong data security, and optimize operational costs based on actual system usage.

### 2. Problem Statement

*Current Challenges*

Traditional gym management systems often struggle during peak hours (6:00–8:00 AM and 5:00–8:00 PM), when many members simultaneously check in or book appointments with personal trainers (PTs). These traffic spikes can cause network congestion or even server outages. In addition, the continuously growing volume of images, invoices, and member check-in logs requires a storage and database solution that is both reliable and cost-effective without maintaining expensive idle infrastructure during off-peak hours.

*Proposed Solution*

Deploy a layered Gym Website architecture on AWS by using Amazon S3 and Amazon CloudFront to accelerate the React.js frontend, AWS WAF to protect against web attacks, and a Node.js + Express backend hosted on Amazon EC2 instances managed by an Auto Scaling Group across two Availability Zones (Multi-AZ). Data storage is separated into Amazon RDS MySQL for relational data, Amazon DynamoDB for check-in logs, and Amazon ElastiCache (Redis) for caching.

*Benefits and Return on Investment (ROI)*

**Technical Benefits**

* Eliminate website downtime during peak hours.
* Reduce API response time to under 100 ms through Redis caching.
* Automatically back up critical business data.

**Business Benefits**

* Significantly reduce infrastructure costs compared to traditional physical servers.
* Lower operating expenses by utilizing AWS Auto Scaling and Free Tier resources.
* Improve member satisfaction and retention through a stable and responsive web application.

### 3. Solution Architecture

The platform adopts a scalable AWS cloud architecture that supports future expansion while ensuring high availability and security. Static content is delivered through Amazon CloudFront and Amazon S3, application logic runs on Amazon EC2, and managed database services provide reliable data storage.

![Architecture](/images/gym.drawio.png)

*AWS Services Used*

* **Amazon Route 53**: DNS service for domain management and request routing.
* **AWS WAF**: Protects the application against SQL injection, malicious requests, and DDoS attacks.
* **Amazon CloudFront**: Content Delivery Network (CDN) that accelerates frontend delivery.
* **Amazon S3**: Stores frontend static files, user images, and uploaded documents.
* **Application Load Balancer (ALB)**: Distributes incoming traffic across backend servers.
* **Amazon EC2 (Node.js + Express, Nginx, PM2)**: Hosts the backend application.
* **Amazon RDS for MySQL**: Stores relational business data with Multi-AZ deployment.
* **Amazon DynamoDB**: Stores check-in logs and application metadata.
* **Amazon ElastiCache (Redis)**: Caches user sessions and frequently accessed data.
* **Amazon CloudWatch & CloudWatch Alarms**: Monitor infrastructure performance and generate alerts.
* **Amazon SNS (Simple Notification Service)**: Sends email or SMS notifications when alarms are triggered.
* **AWS IAM**: Manages authentication, authorization, and resource permissions.

*System Design*

* **Edge & Security Layer**: User requests are routed through Route 53, protected by AWS WAF, and accelerated by CloudFront.
* **Frontend Layer**: The React.js application is hosted as a static website in Amazon S3 and distributed globally by CloudFront.
* **Network & Routing Layer**: A Virtual Private Cloud (VPC) contains public and private subnets. The ALB resides in public subnets, while EC2 instances and databases remain isolated in private subnets.
* **Compute Layer**: Two EC2 instances run the Node.js application in separate Availability Zones and are managed by an Auto Scaling Group.
* **Data Layer**: Amazon RDS stores relational data, DynamoDB stores high-volume logs, and Redis provides high-speed caching.
* **Operations Layer**: CloudWatch continuously monitors system resources. When issues occur, CloudWatch Alarms trigger Amazon SNS notifications for administrators.

### 4. Technical Implementation

* **Frontend**: Deploy the React.js application to Amazon S3 Static Website Hosting and use CloudFront invalidations to publish updates immediately.
* **Backend**: Create a VPC with two Availability Zones using AWS CDK or the AWS Management Console. Deploy EC2 instances running Ubuntu, configure Nginx as a reverse proxy, and use PM2 to manage Node.js processes.
* **Network Security**: Configure Security Groups so that:

  * The ALB accepts HTTP/HTTPS traffic only.
  * EC2 instances accept traffic exclusively from the ALB.
  * RDS and Redis accept connections only from the backend EC2 instances.

### 5. Project Timeline

* **Week 1 – Planning & Design:** Design the database schema and complete the architecture diagram.
* **Week 2 – Frontend & Network Deployment:** Configure the VPC, public/private subnets, deploy the frontend to Amazon S3 and CloudFront, and configure Route 53.
* **Week 3 – Backend Development:** Deploy Node.js, Nginx, and PM2 on EC2. Configure Amazon RDS, DynamoDB, and Redis.
* **Week 4 – Optimization & Testing:** Configure CloudWatch Alarms and Amazon SNS, perform load testing, verify Auto Scaling behavior, and prepare the production environment.

### 6. Estimated Budget

The estimated monthly cost is based on the AWS Pricing Calculator and assumes usage of the AWS Free Tier whenever applicable.

* **Amazon EC2:** Approximately **USD 0.00** under the Free Tier, or approximately **USD 15.00/month** for an Auto Scaling deployment.
* **Amazon RDS MySQL:** Approximately **USD 0.00** under the Free Tier.
* **Amazon S3 & CloudFront:** Approximately **USD 0.50/month**.
* **Amazon DynamoDB & Amazon ElastiCache:** Approximately **USD 1.00/month**.
* **AWS WAF & Amazon Route 53:** Approximately **USD 1.50/month**.
* **Amazon CloudWatch & Amazon SNS:** Approximately **USD 0.00** within Free Tier limits.

**Estimated Total Cost:** Approximately **USD 2.00–18.00 per month**, depending on actual application usage.

### 7. Risk Assessment

**Risk 1:** DDoS or brute-force attacks targeting the login system.

**Mitigation Strategy:** Deploy AWS WAF with rate limiting to block malicious traffic and suspicious bot activity.

**Risk 2:** Failure of an AWS Availability Zone.

**Mitigation Strategy:** Deploy the infrastructure across multiple Availability Zones. If one Availability Zone becomes unavailable, the Application Load Balancer automatically routes traffic to healthy EC2 instances while Amazon RDS Multi-AZ ensures database availability.

**Risk 3:** Unexpected increases in AWS costs.

**Mitigation Strategy:** Configure AWS Budgets and CloudWatch alarms to notify administrators when monthly spending exceeds predefined thresholds (e.g., USD 20).

### 8. Expected Outcomes

* The gym management website operates continuously with high availability.
* The system can handle hundreds or thousands of simultaneous member check-ins and personal trainer bookings during peak hours without performance degradation.
* Member information and payment data are securely protected using AWS best practices, providing a reliable and professional cloud-based solution.
