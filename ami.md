# AWS AMI (Amazon Machine Image) Project

## Overview

This project demonstrates how to create a **Custom Amazon Machine Image (AMI)** from a configured EC2 instance and use it to launch identical instances for application deployment.

A custom AMI, often called a **Golden AMI**, helps standardize server configurations, reduce deployment time, and simplify infrastructure management.

---

## Objective

* Create a fully configured EC2 instance.
* Install and configure an Apache Web Server.
* Create a custom AMI from the configured instance.
* Launch new EC2 instances using the custom AMI.
* Verify application availability.
* Configure and mount additional EBS storage (optional).

---

## Architecture

```text
EC2 Instance
      │
      ▼
Install & Configure Application
      │
      ▼
Create Custom AMI
      │
      ▼
Store AMI in AWS
      │
      ▼
Launch New EC2 Instances
      │
      ▼
Identical Application Environment
```

---

## Prerequisites

Before starting, ensure the following:

* AWS Account
* EC2 Access Permissions
* Key Pair Created
* Security Group Configured

  * SSH (22)
  * HTTP (80)

---

## Step 1: Launch EC2 Instance

Launch an EC2 instance using Amazon Linux.

Example Configuration:

| Parameter     | Value          |
| ------------- | -------------- |
| Instance Name | WebServer      |
| AMI           | Amazon Linux 2 |
| Instance Type | t2.micro       |

Connect to the instance:

```bash
ssh -i key.pem ec2-user@<Public-IP>
```

---

## Step 2: Install Apache Web Server

Install Apache (HTTPD):

```bash
sudo yum install httpd -y
```

Start and enable the service:

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

Verify service status:

```bash
sudo systemctl status httpd
```

---

## Step 3: Create Sample Web Page

Navigate to the web root directory:

```bash
cd /var/www/html
```

Create the index page:

```bash
sudo vi index.html
```

Add the following content:

```html
<html>
<head>
<title>AWS AMI Demo</title>
</head>
<body>
<h1>Welcome to AWS Custom AMI Project</h1>
</body>
</html>
```

Restart Apache:

```bash
sudo systemctl restart httpd
```

---

## Step 4: Verify Web Application

Open a browser and access:

```text
http://<Public-IP>
```

Expected Output:

```text
Welcome to AWS Custom AMI Project
```

---

## Step 5: Create Custom AMI

1. Open AWS Console.
2. Navigate to:

```text
EC2 → Instances
```

3. Select the configured instance.
4. Click:

```text
Actions
→ Image and Templates
→ Create Image
```

5. Enter AMI details:

```text
AMI Name: WebServer-AMI
Description: Apache Configured Server
```

6. Click **Create Image**.

AWS will create a reusable custom AMI from the configured EC2 instance.

---

## Step 6: Launch Instance from Custom AMI

Navigate to:

```text
EC2 → AMIs
```

Select the created AMI and choose:

```text
Launch Instance from AMI
```

Configure:

* Instance Name
* Instance Type
* Key Pair
* Security Group
* Network Settings

Select:

```text
My AMIs → Owned by Me
```

Choose the custom AMI and launch the instance.

---

## Validation

After launching the new instance:

1. Copy the Public IP.
2. Open the browser.
3. Access:

```text
http://<Public-IP>
```

If the same webpage appears, the AMI has been successfully created and deployed.

---

## Additional EBS Volume Configuration (Optional)

### Check Available Disks

```bash
lsblk
```

Example:

```text
xvda
xvdb
```

### Create Mount Directory

```bash
sudo mkdir /data
```

### Mount the Volume

```bash
sudo mount /dev/xvdb /data
```

### Verify Mount

```bash
df -Th
```

Example:

```text
Filesystem      Type  Size Used Avail Mounted on
/dev/xvdb       xfs   10G  1G   9G    /data
```

### Access the Mounted Volume

```bash
cd /data
```

---

## Project Deliverables

* Configured EC2 Instance
* Apache Web Server Installation
* Custom AWS AMI
* New EC2 Instance from AMI
* Application Validation
* Optional EBS Volume Mounting

---

## Learning Outcomes

After completing this project, you will understand:

* AWS EC2 Instance Management
* Amazon Machine Images (AMI)
* Golden AMI Concepts
* Application Deployment Automation
* Infrastructure Standardization
* EBS Storage Management

---

## Benefits of Custom AMI

* Faster Deployment
* Consistent Server Configuration
* Easy Scaling
* Simplified Disaster Recovery
* Reduced Manual Effort
* Reusable Infrastructure Templates

---

## Conclusion

This project demonstrates how to create a custom AWS AMI from a configured EC2 instance and use it to launch identical environments quickly and efficiently. Custom AMIs are a key component of cloud automation, infrastructure standardization, and scalable deployments in AWS.

---

### Author

**AWS DevOps Project – AMI Creation & Deployment**

Created for learning and implementation of AWS EC2 and Amazon Machine Images.
