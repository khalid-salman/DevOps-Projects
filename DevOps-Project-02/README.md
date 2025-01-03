# Deploy a Scalable VPC Architecture on AWS Cloud

![AWS-Cloud](https://imgur.com/AXD50yl.png)

### Table of Contents

1. [Objective](#objective)
2. [Requirements](#requirements)
3. [Preparation](#preparation)
4. [VPC Deployment](#vpc-deployment)
5. [Verification](#verification)

---

## Objective

The goal of this project is to deploy a modular and scalable Virtual Private Cloud (VPC) architecture on AWS. This architecture will support high availability and auto-scaling for application servers, ensuring a robust and efficient network setup.

## Requirements

1. **AWS Account**: You need an [AWS account](https://aws.amazon.com/) to create and manage cloud resources.
2. **Source Code**: Access the [source code](https://github.com/NotHarshhaa/DevOps-Projects/blob/master/DevOps-Project-02/html-web-app) for the web application.

## Preparation

Before deployment, customize the application dependencies on an AWS EC2 instance and create a Golden AMI. The following components need to be installed:

1. **AWS CLI**: Install the AWS Command Line Interface.
2. **Apache Web Server**: Set up the Apache web server.
3. **Git**: Install Git for version control.
4. **CloudWatch Agent**: Install the CloudWatch agent for monitoring.
5. **Custom Memory Metrics**: Push custom memory metrics to CloudWatch.
6. **AWS SSM Agent**: Install the AWS Systems Manager (SSM) agent for instance management.

## VPC Deployment

### Step 1: Build VPC Networks

1. **Bastion Host VPC**: Create a VPC with the CIDR block `192.168.0.0/16` for deploying the Bastion Host.
2. **Application VPC**: Create another VPC with the CIDR block `172.32.0.0/16` for deploying highly available and auto-scalable application servers.

### Step 2: Configure NAT and Internet Gateways

1. **NAT Gateway**: Set up a NAT Gateway in the public subnet and update the private subnet’s route table to route traffic through the NAT Gateway for outbound internet access.
2. **Transit Gateway**: Create a Transit Gateway and associate both VPCs to enable private communication between them.
3. **Internet Gateway**: Create an Internet Gateway for each VPC and update the public subnet’s route table to route traffic through the Internet Gateway for inbound and outbound internet access.

### Step 3: Enable Logging and Monitoring

1. **CloudWatch Log Group**: Create a CloudWatch Log Group with two Log Streams to store VPC Flow Logs for both VPCs.
2. **VPC Flow Logs**: Enable Flow Logs for both VPCs and push the logs to the respective CloudWatch Log Streams.

### Step 4: Deploy Bastion Host

1. **Security Group**: Create a security group for the Bastion Host, allowing SSH access (port 22) from the public internet.
2. **Bastion Host**: Deploy an EC2 instance in the public subnet as the Bastion Host and associate an Elastic IP (EIP) with it.

### Step 5: Configure Application Servers

1. **S3 Bucket**: Create an S3 bucket to store application-specific configurations.
2. **Launch Configuration**: Create a Launch Configuration with the following settings:
   - **Golden AMI**: Use the custom AMI created earlier.
   - **Instance Type**: Use `t2.micro`.
   - **User Data**: Include a script to pull the application code from the repository and start the Apache web server.
   - **IAM Role**: Assign an IAM role with permissions for Session Manager and access to the S3 bucket (avoid granting full S3 access).
   - **Security Group**: Allow port 22 access from the Bastion Host and port 80 from the public internet.
   - **Key Pair**: Use an existing or create a new key pair.
3. **Auto Scaling Group**: Create an Auto Scaling Group with a minimum of 2 and a maximum of 4 instances, distributed across two private subnets in availability zones `1a` and `1b`.
4. **Target Group**: Create a Target Group and associate it with the Auto Scaling Group.
5. **Network Load Balancer**: Deploy a Network Load Balancer (NLB) in the public subnet and add the Target Group as a target.
6. **Route 53**: Update the Route 53 hosted zone with a CNAME record to route traffic to the NLB.

## Verification

1. **Access Private Instances**: As a DevOps engineer, log in to the private instances via the Bastion Host.
2. **AWS Session Manager**: Use AWS Session Manager to access the EC2 shell from the AWS Management Console.
3. **Web Application**: Browse the web application using the domain name from a public internet browser and verify that the page loads correctly.

---

# Hit the Star! ⭐

**If you find this repository useful for learning, please give it a star. Thank you!**

#### Author by [Khalid Salman](https://github.com/khalid-salman)