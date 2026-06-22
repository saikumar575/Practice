# Create an AWS EC2 Instance Using Terraform

## Prerequisites

* An AWS account
* AWS credentials configured locally
* Terraform installed on your machine

---

## Step 1: Configure AWS Credentials

Install and configure the AWS CLI.

```bash
aws configure
```

Provide the following information when prompted:

* AWS Access Key ID
* AWS Secret Access Key
* Default Region (for example: `ap-south-1`)
* Default Output Format (`json`)

Verify your configuration:

```bash
aws sts get-caller-identity
```

If configured correctly, AWS will return details about your account and IAM identity.

---

## Step 2: Create a Terraform Project

Create a new project directory:

```bash
mkdir terraform-ec2
cd terraform-ec2
```

Create a file named `main.tf`.

### Syntax Launch an Amazon Linux 2023 EC2 Instance

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

resource "aws_instance" "my_ec2" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"

  tags = {
    Name = "Terraform-EC2"
  }
}

output "public_ip" {
  value = aws_instance.my_ec2.public_ip
}
```


---

## Step 3: Initialize Terraform

Initialize the Terraform project:

```bash
terraform init
```

This command downloads the required AWS provider plugins.

---

## Step 4: Preview Infrastructure Changes

Review the execution plan before creating resources:

```bash
terraform plan
```

Terraform will display the resources it intends to create.

---

## Step 5: Create the EC2 Instance

Apply the Terraform configuration:

```bash
terraform apply
```

When prompted, type:

```text
yes
```

Terraform will provision the EC2 instance.

---

## Step 6: Check Outputs

Retrieve the outputs defined in your configuration:

```bash
terraform output
```

Example output:

```text
public_ip = "13.xx.xx.xx"
```

---

## Step 7: Destroy Resources

To avoid unnecessary AWS charges, destroy resources when no longer needed:

```bash
terraform destroy
```

Confirm the operation by typing:

```text
yes
```

Terraform will remove the EC2 instance and associated resources.


### Benefits

* Automatically uses the latest Amazon Linux 2023 AMI
* Eliminates manual AMI updates
* Improves maintainability and security
* Works across future AMI releases

---

## Summary

Terraform simplifies infrastructure provisioning by allowing you to define AWS resources as code. The workflow is:

1. Configure AWS credentials
2. Create Terraform configuration
3. Initialize Terraform
4. Review the execution plan
5. Apply the configuration
6. Verify outputs
7. Destroy resources when finished
