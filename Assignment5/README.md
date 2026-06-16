# Assignment 05 - Implement Infrastructure with Terraform Modules

## Overview

This project demonstrates how to implement AWS infrastructure using reusable Terraform modules. The infrastructure is organized into separate modules for VPC, Subnet, Security Group, and EC2 Instance. Remote state management is configured using Amazon S3, and state locking is implemented using DynamoDB.

---

## Objectives

* Create reusable Terraform modules.
* Deploy AWS infrastructure using Terraform.
* Implement Terraform Remote State Management using Amazon S3.
* Enable State Locking using DynamoDB.
* Follow Terraform best practices for modularity and maintainability.

---

## Project Structure

```text
Task-1/
│
├── backend.tf
├── provider.tf
├── main.tf
├── variables.tf
├── outputs.tf
│
└── modules/
    ├── vpc/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    ├── subnet/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    ├── security-group/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    └── ec2/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

---

## Technologies Used

* Terraform v1.15.5
* AWS CLI
* Amazon EC2
* Amazon VPC
* Amazon S3
* Amazon DynamoDB
* Visual Studio Code

---

## Infrastructure Components

### VPC Module

Creates a custom Virtual Private Cloud (VPC).

### Subnet Module

Creates a subnet within the VPC.

### Security Group Module

Creates a security group with required inbound and outbound rules.

### EC2 Module

Launches an EC2 instance within the created subnet and security group.

---

## Remote State Management

Terraform state is stored remotely in an S3 bucket.

### S3 Bucket

```text
Bucket Name: ratnu-terraform-state-2026
Region: ap-south-1
```

### Backend Configuration

```hcl
terraform {
  backend "s3" {
    bucket         = "ratnu-terraform-state-2026"
    key            = "terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

---

## State Locking

To prevent concurrent Terraform executions, DynamoDB state locking is enabled.

### DynamoDB Table

```text
Table Name: terraform-locks
Partition Key: LockID
```

---

## Commands Executed

### Initialize Terraform

```bash
terraform init
```

### Validate Configuration

```bash
terraform validate
```

### Preview Infrastructure Changes

```bash
terraform plan
```

### Deploy Infrastructure

```bash
terraform apply
```

### Destroy Infrastructure (Optional)

```bash
terraform destroy
```

---

## Outputs

After successful deployment, Terraform generates:

* VPC ID
* Subnet ID
* Security Group ID

Example:

```text
vpc_id    = vpc-xxxxxxxx
subnet_id = subnet-xxxxxxxx
sg_id     = sg-xxxxxxxx
```

---

## Results

Successfully deployed:

* VPC
* Subnet
* Security Group
* EC2 Instance

Successfully configured:

* Terraform Modules
* Remote State Management using S3
* State Locking using DynamoDB

---

## Conclusion

This project demonstrates Infrastructure as Code (IaC) using Terraform with a modular architecture. Terraform modules improve code reusability and maintainability, while Amazon S3 and DynamoDB provide secure remote state management and state locking for collaborative environments.
