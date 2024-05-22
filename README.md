# Static Website with High Availability & Scalability on AWS

## Overview

This project demonstrates how to host a static website on AWS with high availability and scalability. It utilizes multiple AWS services to ensure the website can handle traffic variations and remain available even in the event of failures. The setup includes EC2 instances for hosting the website, an Application Load Balancer (ALB) for distributing traffic, an Auto Scaling Group (ASG) for scaling instances, and Route 53 for domain name management.

## Working Architecture
![Architecture Diagram](aws-project-1/Working Architecture.png) <!-- Replace with the path to your diagram -->

## AWS Services Used
* **EC2 Instances**: Virtual servers to host your website.
* **Application Load Balancer (ALB)**: Distributes incoming traffic across multiple instances to ensure high availability.
* **Auto Scaling Group (ASG)**: Automatically adjusts the number of EC2 instances based on traffic demand.
* **Route 53**: DNS web service to manage your domain name.

## Step-by-Step Instructions

### 1. EC2 Instances

EC2 (Elastic Compute Cloud) instances are virtual servers in AWS that host your applications. In this project, they will serve your static website.

**Steps**:
1. **Create an EC2 Instance**:
    - AMI: Amazon Linux
    - Instance Type: t2.micro
    - Key Pair: Select an existing key pair or create a new one for SSH access.

2. **Connect to the Instance via SSH**:
    ```bash
    ssh -i /path/to/key-pair.pem ec2-user@<instance-public-dns>
    ```

3. **Update and Install Apache HTTP Server**:
    ```bash
    sudo yum update -y
    sudo yum install httpd -y
    sudo systemctl start httpd
    sudo systemctl enable httpd
    ```

4. **Deploy Your Static Website**:
    - Create a temporary directory and download your website content:
      ```bash
      mkdir temp
      cd temp
      wget <zip-file-link>  # Replace with the actual link
      unzip <zip-file-name> # Replace with the actual file name
      cd <unzipped-folder>  # Replace with the actual folder name
      ```
    - Move content to the Apache default root directory:
      ```bash
      sudo mv * /var/www/html/
      ```

5. **Create an AMI**:
    - Go to the EC2 console, select your instance, and choose `Create Image` from the `Actions` menu. This AMI will be used in the Auto Scaling Group.

### 2. Application Load Balancer (ALB)

An ALB distributes incoming traffic across multiple EC2 instances to ensure high availability and fault tolerance.

**Steps**:
1. **Create an ALB**:
    - Select the appropriate VPC and Availability Zones.
    - Configure security groups to allow necessary traffic (typically HTTP/HTTPS).

2. **Create a Target Group**:
    - Select the instances you created in the previous step.

3. **Register Targets**:
    - Ensure the instances in your target group show as healthy.

4. **Test ALB**:
    - Copy the DNS name of the ALB and paste it into your browser to verify the website is accessible.

### 3. Auto Scaling Group (ASG)

An ASG automatically adjusts the number of EC2 instances in response to traffic patterns, ensuring the application can handle varying loads efficiently
