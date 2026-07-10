---

title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
--------------------

# Configuring Secure Connectivity from a Private Subnet to Amazon S3 Using a VPC Endpoint

## Overview

In a typical cloud architecture such as the **Gym Website** or an **IoT Weather Monitoring System**, backend services running on Amazon EC2, AWS Glue jobs, or AWS Lambda functions are deployed within **private subnets** without public IP addresses. By default, these services require Internet access through a NAT Gateway or Internet Gateway to upload or download files from Amazon S3. This approach increases both security risks and network costs.

This workshop demonstrates how to configure an **Amazon VPC Gateway Endpoint** for Amazon S3, allowing resources within a VPC to communicate with S3 through the AWS private network. The solution is further secured using **IAM policies** and **Amazon S3 bucket policies**, ensuring that data never traverses the public Internet.

## Prerequisites

Before starting this workshop, prepare the following AWS resources:

* **One Amazon VPC** configured with at least:

  * One **Public Subnet** (used as a bastion host or management subnet if required).
  * One **Private Subnet** containing no direct route (`0.0.0.0/0`) to an Internet Gateway.
* **One Amazon EC2 instance** (Ubuntu or Amazon Linux 2) deployed in the Private Subnet with the **AWS CLI** installed.
* **One IAM Role** attached to the EC2 instance with permissions to access Amazon S3 (for example, `AmazonS3ReadOnlyAccess` or a custom IAM policy).
* **One Amazon S3 bucket** created for testing file uploads and downloads.

## Architecture Description

Before a VPC Endpoint is configured, an EC2 instance located in a Private Subnet must access Amazon S3 through a **NAT Gateway**, which forwards traffic to the public S3 endpoint over the Internet.

After creating an **Amazon S3 Gateway VPC Endpoint**, the endpoint is associated directly with the **Route Table** of the Private Subnet. Any traffic destined for Amazon S3 is automatically routed through the **AWS Backbone Network** instead of the public Internet. This private communication path improves security, reduces network latency, and eliminates NAT Gateway data transfer charges for S3 traffic.

As a result, backend applications can securely upload and download files from Amazon S3 while remaining completely isolated within the private network.
