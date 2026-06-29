# AWS VPC Creation Using Terraform

## 📌 Project Overview

This project demonstrates how to create an AWS Virtual Private Cloud (VPC) using **Terraform** on an Amazon Linux EC2 instance. It covers installing Terraform, configuring AWS credentials, provisioning infrastructure, and verifying the deployment.

---

## 📖 Objectives

* Install Terraform on Amazon Linux.
* Configure AWS CLI authentication.
* Create an AWS VPC using Terraform.
* Verify the deployed infrastructure.
* Manage infrastructure using Infrastructure as Code (IaC).

---

## 🛠️ Prerequisites

* AWS Account
* IAM User with VPC permissions
* Amazon Linux EC2 Instance
* AWS CLI installed
* Internet connectivity

---

## 📂 Project Structure

```text
terraform-vpc/
│── main.tf
│── terraform.tfstate
│── terraform.tfstate.backup
└── .terraform/
```

---

## 🚀 Installation Steps

### Step 1: Install Required Packages

```bash
sudo yum install -y yum-utils shadow-utils
```

### Step 2: Add the HashiCorp Repository

```bash
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
```

### Step 3: Install Terraform

```bash
sudo yum install terraform
```

### Step 4: Verify Terraform Installation

```bash
terraform --version
```

---

## ⚙️ Configure AWS Credentials

```bash
aws configure
```

Provide the following details:

```text
AWS Access Key ID
AWS Secret Access Key
Default Region
Default Output Format
```

Verify authentication:

```bash
aws sts get-caller-identity
```

---

## 📝 Terraform Configuration

Create a file named **main.tf**.

```hcl
provider "aws" {
  region = "ap-south-1"
}

resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "main-vpc"
  }
}

resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.1.0/24"
  availability_zone       = "ap-south-1a"
  map_public_ip_on_launch = true

  tags = {
    Name = "public-subnet"
  }
}
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main.id

  tags = {
    Name = "main-igw"
  }
}

resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }

  tags = {
    Name = "public-route-table"
  }
}

resource "aws_route_table_association" "public" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public.id
} 
```

---

## ▶️ Initialize Terraform

```bash
terraform init
```

Downloads the required provider plugins and initializes the working directory.

---

## 📋 Review the Execution Plan

```bash
terraform plan
```

This command previews the infrastructure changes without creating resources.

---

## ☁️ Create the VPC

```bash
terraform apply
```

When prompted, type:

```text
yes
```

Terraform will create the VPC in your AWS account.

---

## ✅ Verify the Deployment

1. Sign in to the AWS Management Console.
2. Navigate to **VPC Dashboard**.
3. Select **Your VPCs**.
4. Verify that the VPC named **Terraform-VPC** has been created.

---

## 🧹 Destroy the Infrastructure

To remove the VPC and related resources:

```bash
terraform destroy
```

Type:

```text
yes
```

---

## 📜 Commands Used

```bash
sudo yum install -y yum-utils shadow-utils

sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo

sudo yum install terraform

terraform --version

vim main.tf

aws configure

aws sts get-caller-identity

terraform init

terraform plan

terraform apply

terraform destroy
```

---

## 🎯 Learning Outcomes

* Installed Terraform on Amazon Linux.
* Configured AWS CLI credentials.
* Provisioned an AWS VPC using Terraform.
* Validated infrastructure deployment.
* Learned Infrastructure as Code (IaC) concepts.
* Managed AWS resources using Terraform lifecycle commands.

---

## 📚 Technologies Used

* Terraform
* AWS VPC
* AWS CLI
* Amazon Linux
* Infrastructure as Code (IaC)

---
