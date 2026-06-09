# Create an AWS Auto Scaling Group (ASG) from an Existing EC2 Instance

This guide explains how to create an Auto Scaling Group using an existing EC2 instance.

---

# Prerequisites

Before starting, ensure you have:

* An existing EC2 instance running successfully
* IAM permissions for:

  * EC2
  * Auto Scaling
  * Elastic Load Balancing (optional)
  * CloudWatch
* A VPC and Subnets available

---

# Step 1: Create an AMI from the Existing EC2 Instance

The Auto Scaling Group launches new instances from an Amazon Machine Image (AMI).

### 1. Login to AWS Console

Open:

```text
AWS Console → EC2 Dashboard
```

### 2. Select Your Existing Instance

Navigate to:

```text
Instances → Select Instance
```

### 3. Create Image

Click:

```text
Actions
 └── Image and Templates
      └── Create Image
```

### 4. Enter Details

Example:

```text
Image Name: web-server-ami
Image Description: Production Web Server
```

### 5. Create AMI

Click:

```text
Create Image
```

### 6. Verify AMI Creation

Navigate to:

```text
EC2 → AMIs
```

Wait until status becomes:

```text
Available
```

---

# Step 2: Create a Launch Template

Launch Templates define how new EC2 instances will be created.

### 1. Open Launch Templates

Navigate to:

```text
EC2 → Launch Templates
```

### 2. Click Create Launch Template

Enter:

```text
Launch Template Name:
web-server-template
```

### 3. Select AMI

Choose the AMI created in Step 1.

Example:

```text
web-server-ami
```

### 4. Select Instance Type

Example:

```text
t3.micro
```

### 5. Select Key Pair

Choose your existing key pair.

Example:

```text
my-keypair
```

### 6. Select Security Group

Example:

```text
web-server-sg
```

Allow:

```text
HTTP  : 80
HTTPS : 443
SSH   : 22
```

### 7. Configure Storage

Example:

```text
Root Volume: 20 GB GP3
```

### 8. Create Launch Template

Click:

```text
Create Launch Template
```

---

# Step 3: Create Auto Scaling Group

### 1. Open Auto Scaling Groups

Navigate to:

```text
EC2 → Auto Scaling Groups
```

### 2. Click Create Auto Scaling Group

### 3. Enter ASG Name

Example:

```text
web-server-asg
```

### 4. Select Launch Template

Choose:

```text
web-server-template
```

Click:

```text
Next
```

---

# Step 4: Configure Network

### Select VPC

Example:

```text
Production-VPC
```

### Select Multiple Subnets

Example:

```text
Subnet-A (us-east-1a)
Subnet-B (us-east-1b)
```

Using multiple Availability Zones improves availability.

Click:

```text
Next
```

---

# Step 5: Attach Load Balancer (Recommended)

### Option A: Existing Load Balancer

Select:

```text
Attach to Existing Load Balancer
```

Choose:

```text
Application Load Balancer (ALB)
```

Select Target Group.

Example:

```text
web-server-tg
```

### Option B: Skip

If not required, continue without a load balancer.

---

# Step 6: Configure Health Checks

Enable:

```text
EC2 Health Checks
```

Recommended:

```text
Elastic Load Balancer Health Checks
```

Health Check Grace Period:

```text
300 Seconds
```

Click:

```text
Next
```

---

# Step 7: Configure Group Size

Example:

```text
Desired Capacity : 1
Minimum Capacity : 1
Maximum Capacity : 5
```

Meaning:

| Setting | Description                     |
| ------- | ------------------------------- |
| Min     | Always keep at least 1 instance |
| Desired | Start with 1 instance           |
| Max     | Scale up to 5 instances         |

---

# Step 8: Configure Scaling Policy

Choose:

```text
Target Tracking Scaling Policy
```

Metric:

```text
Average CPU Utilization
```

Target Value:

```text
50%
```

Behavior:

```text
CPU > 50% → Launch New Instance
CPU < 50% → Remove Extra Instance
```

Click:

```text
Next
```

---

# Step 9: Review and Create

Review:

```text
Launch Template
VPC
Subnets
Capacity
Scaling Policies
```

Click:

```text
Create Auto Scaling Group
```

---

# Step 10: Verify Auto Scaling Group

Navigate to:

```text
EC2 → Auto Scaling Groups
```

Select:

```text
web-server-asg
```

Verify:

```text
Desired Capacity = 1
Current Capacity = 1
Healthy Instances = 1
```

---

# Step 11: Test Auto Scaling

### Generate CPU Load

Connect to EC2:

```bash
ssh -i my-key.pem ec2-user@<public-ip>
```

Install stress tool:

```bash
sudo yum install stress -y
```

Generate load:

```bash
stress --cpu 4 --timeout 600
```

### Monitor

Navigate to:

```text
CloudWatch → Metrics
```

Watch CPU increase.

After threshold is crossed:

```text
ASG launches additional EC2 instances automatically.
```

---

# Architecture Diagram

```text
Internet
    |
    v
Application Load Balancer
    |
    v
+---------------------------+
| Auto Scaling Group        |
|---------------------------|
| EC2 Instance 1            |
| EC2 Instance 2            |
| EC2 Instance 3            |
+---------------------------+
    |
    v
CloudWatch Metrics
    |
    v
Scaling Policies
```

---

# Recommended Production Flow

```text
Existing EC2 Instance
          |
          v
      Create AMI
          |
          v
 Create Launch Template
          |
          v
 Create Auto Scaling Group
          |
          v
 Attach Load Balancer
          |
          v
 Configure Scaling Policies
          |
          v
 Production Ready Setup
```
