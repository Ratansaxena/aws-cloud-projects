# Assignment 4 - Infrastructure Implementation using Terraform Ia

## Problem Statement

The objective of this assignment is to design and implement cloud infrastructure for an application using Terraform Infrastructure as Code (IaC). The infrastructure should be created based on a predefined architecture diagram and follow industry best practices for automation, consistency, scalability, and maintainability.

Additionally, Terraform Remote State Management should be implemented using Amazon S3 to securely store and manage the Terraform state file.

---

# Objective

* Design infrastructure architecture for the application.
* Implement infrastructure using Terraform IaC.
* Automate resource provisioning on AWS.
* Maintain consistent Terraform provider versioning.
* Store Terraform state remotely using Amazon S3.
* Follow Infrastructure as Code best practices.

---

# Infrastructure Architecture

## Architecture Diagram

```text id="mjlwm8"
                         INTERNET
                             │
                             ▼
                   ┌──────────────────┐
                   │ Application Load │
                   │     Balancer     │
                   └──────────────────┘
                             │
            ┌────────────────┴────────────────┐
            │                                 │
            ▼                                 ▼
     Public Subnet-A                  Public Subnet-B
            │                                 │
            ▼                                 ▼
      NAT Gateway                      NAT Gateway
            │                                 │
            └──────────────┬──────────────────┘
                           │
                           ▼
                ┌────────────────────┐
                │ Auto Scaling Group │
                └────────────────────┘
                           │
          ┌────────────────┴────────────────┐
          │                                 │
          ▼                                 ▼
   EC2 Instance-1                    EC2 Instance-2
   Private Subnet-A                  Private Subnet-B
          │                                 │
          └──────────────┬──────────────────┘
                         │
                         ▼
                   Application
```

---

# AWS Components Used

## VPC

A dedicated Virtual Private Cloud (VPC) provides network isolation and secure communication between resources.

### Configuration

* CIDR Block: 10.0.0.0/16
* Multi Availability Zone deployment

---

## Public Subnets

Used to host internet-facing resources.

### Resources

* Application Load Balancer
* NAT Gateway

---

## Private Subnets

Used to host application servers securely.

### Resources

* EC2 Instances
* Auto Scaling Group

---

## Internet Gateway

Provides internet connectivity for public resources.

---

## NAT Gateway

Allows private EC2 instances to access the internet securely for:

* Package installation
* Software updates
* Repository access

---

## Application Load Balancer

Distributes incoming traffic across multiple EC2 instances.

### Features

* High Availability
* Health Checks
* Traffic Distribution
* Fault Tolerance

---

## Auto Scaling Group

Automatically scales EC2 instances based on application demand.

### Configuration

* Minimum Capacity: 2
* Desired Capacity: 2
* Maximum Capacity: 4

---

## Security Groups

### Load Balancer Security Group

Allows:

* HTTP (80)

### EC2 Security Group

Allows:

* HTTP (80) from ALB
* SSH (22) from Admin IP

---

# Terraform Implementation

Infrastructure was provisioned using Terraform.

## Terraform Files

```text id="2h1hkw"
terraform-project/
│
├── provider.tf
├── main.tf
├── variables.tf
├── outputs.tf
├── backend.tf
│
└── terraform.tfvars
```

---

# Terraform Provider Configuration

```hcl id="5io4o0"
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

---

# Terraform State Management

Terraform state is stored remotely using Amazon S3.

## Benefits

* Centralized state management
* Team collaboration
* State file backup
* Improved reliability

---

## S3 Backend Configuration

```hcl id="uimov6"
terraform {
  backend "s3" {
    bucket  = "terraform-state-bucket"
    key     = "terraform.tfstate"
    region  = "ap-south-1"
    encrypt = true
  }
}
```

---

# Terraform Commands Used

## Initialize Terraform

```bash id="m1izlw"
terraform init
```

## Validate Configuration

```bash id="r6sl6l"
terraform validate
```

## Generate Execution Plan

```bash id="qv2rj6"
terraform plan
```

## Apply Infrastructure

```bash id="zj9nb3"
terraform apply
```

## Destroy Infrastructure

```bash id="2h7qpf"
terraform destroy
```

---

# Expected Outcome

After successful deployment:

* VPC is created.
* Public and Private Subnets are provisioned.
* Load Balancer is configured.
* Auto Scaling Group is configured.
* EC2 instances are launched.
* Security Groups are applied.
* Terraform state is stored in Amazon S3.

---

# Best Practices Followed

* Infrastructure as Code (IaC)
* Provider Version Pinning
* Remote State Management
* Modular Configuration
* Secure Network Segmentation
* Auto Scaling for High Availability
* Load Balancing for Fault Tolerance

---

# Conclusion

This project successfully demonstrates the implementation of AWS infrastructure using Terraform Infrastructure as Code. The solution provides scalability, high availability, security, and maintainability while leveraging Amazon S3 for remote Terraform state management. By following Terraform best practices and AWS architectural principles, the infrastructure is reliable, reusable, and production-ready.
