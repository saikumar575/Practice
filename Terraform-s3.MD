# Create an AWS S3 Bucket Using Terraform

---

# Prerequisites

* An AWS account
* AWS CLI installed and configured
* Terraform installed
* Appropriate IAM permissions to create S3 buckets

Verify AWS credentials:

```bash
aws sts get-caller-identity
```

---

# Project Structure

Create a new project directory:

```bash
mkdir terraform-s3
cd terraform-s3
```

Create the following file:

```text
terraform-s3/
└── main.tf
```

---

# Step 1: Create Terraform Configuration

Create a file named `main.tf`.

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }

  required_version = ">= 1.5.0"
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "demo_bucket" {
  bucket = "my-unique-terraform-demo-bucket-2026"

  tags = {
    Name        = "Terraform-S3-Bucket"
    Environment = "Dev"
    ManagedBy   = "Terraform"
  }
}
```

> **Note:** S3 bucket names must be globally unique across all AWS accounts.

---

# Step 2: Initialize Terraform

Initialize the project and download the AWS provider.

```bash
terraform init
```

Expected output:

```text
Terraform has been successfully initialized!
```

---

# Step 3: Validate Configuration

Validate the Terraform configuration.

```bash
terraform validate
```

Expected output:

```text
Success! The configuration is valid.
```

---

# Step 4: Preview Changes

Generate an execution plan.

```bash
terraform plan
```

Terraform displays the resources it intends to create.

Example:

```text
Plan: 1 to add, 0 to change, 0 to destroy.
```

---

# Step 5: Create the S3 Bucket

Apply the configuration.

```bash
terraform apply
```

Confirm when prompted:

```text
yes
```

Example output:

```text
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

# Step 6: Verify the Bucket

Using AWS CLI:

```bash
aws s3 ls
```

Or check the bucket in the AWS Management Console under S3.

---

# Step 7: Output Bucket Information

Add the following output block to `main.tf`.

```hcl
output "bucket_name" {
  value = aws_s3_bucket.demo_bucket.bucket
}

output "bucket_arn" {
  value = aws_s3_bucket.demo_bucket.arn
}
```

Run:

```bash
terraform apply
```

View outputs:

```bash
terraform output
```

Example:

```text
bucket_name = "my-unique-terraform-demo-bucket-2026"
bucket_arn  = "arn:aws:s3:::my-unique-terraform-demo-bucket-2026"
```

---

# Enable Versioning

Versioning helps protect objects from accidental deletion or overwrites.

```hcl
resource "aws_s3_bucket" "demo_bucket" {
  bucket = "my-unique-terraform-demo-bucket-2026"
}

resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.demo_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}
```

---

# Enable Server-Side Encryption

Encrypt objects stored in the bucket.

```hcl
resource "aws_s3_bucket_server_side_encryption_configuration" "encryption" {
  bucket = aws_s3_bucket.demo_bucket.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}
```

---

# Block Public Access

Recommended security practice.

```hcl
resource "aws_s3_bucket_public_access_block" "block_public" {
  bucket = aws_s3_bucket.demo_bucket.id

  block_public_acls       = true
  ignore_public_acls      = true
  block_public_policy     = true
  restrict_public_buckets = true
}
```

---

# Complete Production-Ready Example

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 6.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "demo_bucket" {
  bucket = "my-unique-terraform-demo-bucket-2026"

  tags = {
    Name        = "Terraform-S3"
    Environment = "Production"
  }
}

resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.demo_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "encryption" {
  bucket = aws_s3_bucket.demo_bucket.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

resource "aws_s3_bucket_public_access_block" "public_access" {
  bucket = aws_s3_bucket.demo_bucket.id

  block_public_acls       = true
  ignore_public_acls      = true
  block_public_policy     = true
  restrict_public_buckets = true
}

output "bucket_name" {
  value = aws_s3_bucket.demo_bucket.bucket
}

output "bucket_arn" {
  value = aws_s3_bucket.demo_bucket.arn
}
```

---

# Destroy Resources

When the bucket is no longer needed:

```bash
terraform destroy
```

Confirm:

```text
yes
```

Terraform removes the S3 bucket and related resources.

> **Important:** If the bucket contains objects, Terraform may fail to delete it. Empty the bucket first or configure force deletion if appropriate.

---

# Summary

Terraform makes S3 bucket creation simple and repeatable:

1. Configure AWS credentials
2. Define the S3 bucket in Terraform
3. Initialize Terraform
4. Validate the configuration
5. Review the execution plan
6. Apply the configuration
7. Manage bucket settings such as versioning, encryption, and public access controls
8. Destroy resources when no longer required

