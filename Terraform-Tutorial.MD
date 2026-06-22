# Terraform Overview

## What is Terraform?

Terraform is an Infrastructure as Code (IaC) tool developed by [HashiCorp](https://www.hashicorp.com?utm_source=chatgpt.com) that enables you to define, provision, and manage infrastructure using declarative configuration files.

Instead of manually creating resources through cloud consoles, Terraform allows you to describe infrastructure in code and automatically create, update, or delete resources.

Terraform supports multiple platforms, including:

* Amazon Web Services (AWS)
* Microsoft Azure
* Google Cloud Platform (GCP)
* Kubernetes
* VMware
* Oracle Cloud
* GitHub
* Many other providers

---

## Why Use Terraform?

### Infrastructure as Code (IaC)

Infrastructure is defined in code, making deployments repeatable and version-controlled.

### Automation

Reduces manual work and human errors by automating infrastructure provisioning.

### Multi-Cloud Support

Manage resources across multiple cloud providers using a single tool.

### Consistency

Ensures environments remain consistent across development, testing, and production.

### Version Control

Terraform configuration files can be stored in Git repositories for collaboration and auditing.

---

# Terraform Architecture

Terraform follows a simple but powerful architecture consisting of several key components.

## Terraform Architecture Diagram

```text
+---------------------+
| Terraform Config    |
| (*.tf Files)        |
+----------+----------+
           |
           v
+---------------------+
| Terraform Core      |
+----------+----------+
           |
           v
+---------------------+
| Provider Plugins    |
| AWS / Azure / GCP   |
+----------+----------+
           |
           v
+---------------------+
| Cloud Resources     |
| EC2, VPC, S3, etc.  |
+---------------------+
```

---

## Components of Terraform Architecture

### 1. Terraform Configuration Files

Configuration files contain the desired infrastructure state.

Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

Files are typically written in:

* HCL (HashiCorp Configuration Language)
* `.tf` extension

---

### 2. Terraform Core

Terraform Core is the main engine responsible for:

* Reading configuration files
* Building execution plans
* Managing state files
* Determining resource dependencies
* Coordinating provider plugins

Key commands:

```bash
terraform init
terraform plan
terraform apply
terraform destroy
```

---

### 3. Providers

Providers act as bridges between Terraform and external platforms.

Examples:

* AWS Provider
* Azure Provider
* Google Cloud Provider
* Kubernetes Provider
* GitHub Provider

Example:

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

Terraform downloads providers during:

```bash
terraform init
```

---

### 4. Resources

Resources represent infrastructure components managed by Terraform.

Examples:

* EC2 Instances
* VPCs
* Security Groups
* S3 Buckets
* RDS Databases

Example:

```hcl
resource "aws_s3_bucket" "demo" {
  bucket = "my-demo-bucket"
}
```

---

### 5. State File

Terraform maintains a state file named:

```text
terraform.tfstate
```

The state file tracks:

* Existing infrastructure
* Resource IDs
* Current configuration mappings

Benefits:

* Faster execution
* Change tracking
* Dependency management

Best Practice:

Store state remotely using:

* AWS S3
* Azure Storage Account
* Terraform Cloud

Example:

```hcl
terraform {
  backend "s3" {
    bucket = "terraform-state-bucket"
    key    = "prod/terraform.tfstate"
    region = "ap-south-1"
  }
}
```

---

### 6. Execution Plan

Terraform compares:

* Desired state (configuration files)
* Current state (state file)

Then generates an execution plan.

Command:

```bash
terraform plan
```

The plan shows:

* Resources to create
* Resources to modify
* Resources to destroy

---

### 7. Dependency Graph

Terraform automatically determines resource dependencies.

Example:

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "public" {
  vpc_id = aws_vpc.main.id
}
```

Terraform creates the VPC before creating the subnet.

---

# Terraform Workflow

```text
Write Configuration
        |
        v
terraform init
        |
        v
terraform validate
        |
        v
terraform plan
        |
        v
terraform apply
        |
        v
Infrastructure Created
        |
        v
terraform destroy
```

---

# Terraform Lifecycle

### Initialize

```bash
terraform init
```

Downloads providers and initializes the working directory.

### Validate

```bash
terraform validate
```

Checks configuration syntax and structure.

### Plan

```bash
terraform plan
```

Creates an execution plan.

### Apply

```bash
terraform apply
```

Creates or updates infrastructure.

### Destroy

```bash
terraform destroy
```

Removes managed infrastructure.

---

# Key Terraform Files

| File              | Purpose                           |
| ----------------- | --------------------------------- |
| main.tf           | Main infrastructure configuration |
| variables.tf      | Input variable definitions        |
| outputs.tf        | Output values                     |
| terraform.tfvars  | Variable values                   |
| providers.tf      | Provider configuration            |
| terraform.tfstate | Infrastructure state tracking     |

---

# Benefits of Terraform

* Open-source and widely adopted
* Supports multi-cloud environments
* Infrastructure as Code (IaC)
* Automatic dependency management
* Version-controlled infrastructure
* Reusable modules
* Consistent deployments
* Large provider ecosystem

---

# Conclusion

Terraform enables teams to automate infrastructure provisioning through code. Its architecture consists of Terraform Core, Providers, Resources, and State Management, allowing organizations to build scalable, repeatable, and reliable cloud infrastructure across multiple platforms.
