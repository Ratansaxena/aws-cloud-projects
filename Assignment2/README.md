# Assignment 02 - AWS S3 Bucket and IAM User Creation Using Terraform

## Problem Statement

Organizations often require secure and controlled access to cloud storage for storing application data, backups, logs, and other critical assets. Managing AWS resources manually can lead to inconsistencies, security risks, and operational overhead.

The objective of this assignment is to automate the provisioning of an Amazon S3 Bucket and IAM User using Terraform Infrastructure as Code (IaC). The solution should ensure secure access management by granting only the necessary permissions to the IAM user for interacting with the S3 bucket.

Additionally, the S3 bucket should support versioning to protect against accidental deletion or modification of objects, while proper tagging should be implemented for resource identification and management.

---

# Objective

The objective of this assignment is to:

* Create an Amazon S3 Bucket using Terraform.
* Enable Versioning on the S3 Bucket.
* Apply Tags for resource identification.
* Create an IAM User using Terraform.
* Create a custom IAM Policy for S3 access.
* Attach the IAM Policy to the IAM User.
* YES  Provide secure and controlled access to the S3 Bucket.

---

# Architecture Diagram

```text
                    ┌──────────────────┐
                    │   Terraform IaC  │
                    └─────────┬────────┘
                              │
                              ▼
                    ┌──────────────────┐
                    │    AWS Account   │
                    └─────────┬────────┘
                              │
              ┌───────────────┴───────────────┐
              │                               │
              ▼                               ▼
     ┌────────────────┐              ┌────────────────┐
     │   S3 Bucket    │              │    IAM User    │
     │  Versioning ON │              │ terraform-user │
     └────────────────┘              └───────┬────────┘
                                              │
                                              ▼
                                   ┌────────────────────┐
                                   │    IAM Policy      │
                                   │ S3 Access Policy   │
                                   └────────────────────┘
                                              │
                                              ▼
                                   Access to S3 Bucket
```

---

# AWS Services Used

## Amazon S3

Used for storing objects, files, application assets, and backups.

### Features Implemented

* Bucket Creation
* Versioning Enabled
* Resource Tagging

---

## AWS IAM

Used for Identity and Access Management.

### Features Implemented

* IAM User Creation
* Custom IAM Policy
* Policy Attachment

---

# Terraform Resources Used

### S3 Resources

```hcl
aws_s3_bucket
aws_s3_bucket_versioning
```

### IAM Resources

```hcl
aws_iam_user
aws_iam_policy
aws_iam_user_policy_attachment
```

---

# S3 Bucket Configuration

## Bucket Features

* Versioning Enabled
* Secure Access Control
* Tagged Resources
* Centralized Storage

### Tags Applied

```text
Name        = Terraform-S3-Bucket
Environment = Dev
```

---

# IAM User Configuration

## IAM User

```text
terraform-user
```

### Responsibilities

* Access S3 Bucket
* Upload Objects
* Download Objects
* List Bucket Contents

---

# IAM Policy Permissions

The IAM User is granted the following permissions:

```json
{
  "Effect": "Allow",
  "Action": [
    "s3:ListBucket",
    "s3:GetObject",
    "s3:PutObject",
    "s3:DeleteObject"
  ],
  "Resource": "*"
}
```

### Allowed Actions

* List Bucket
* Upload Files
* Download Files
* Delete Files

---

# Policy Attachment Flow

```text
IAM User
    │
    ▼
IAM Policy
    │
    ▼
S3 Bucket Access
```

The IAM policy is attached directly to the IAM user, enabling controlled interaction with the S3 bucket.

---

# Terraform Project Structure

```text
terraform-s3-iam/
│
├── provider.tf
├── main.tf
├── variables.tf
├── outputs.tf
│
└── terraform.tfvars
```

---

# Terraform Workflow

## Initialize Terraform

```bash
terraform init
```

## Validate Configuration

```bash
terraform validate
```

## Generate Execution Plan

```bash
terraform plan
```

## Deploy Infrastructure

```bash
terraform apply
```

## Destroy Infrastructure

```bash
terraform destroy
```

---

# Security Best Practices Followed

* Principle of Least Privilege
* Versioning Enabled for Data Protection
* IAM-Based Access Control
* Resource Tagging for Governance
* Infrastructure as Code (IaC)
* Reusable and Maintainable Terraform Configuration

---

# Expected Outcome

After successful deployment:

* An Amazon S3 Bucket is created.
* Versioning is enabled on the bucket.
* Tags are applied to the bucket.
* An IAM User is created.
* A custom IAM Policy is created.
* The IAM Policy is attached to the IAM User.
* The IAM User can securely interact with the S3 Bucket according to assigned permissions.

---

# Benefits of the Solution

* Automated Infrastructure Provisioning
* Improved Security and Access Control
* Better Resource Management
* Version-Controlled Object Storage
* Reduced Manual Configuration Effort
* Consistent and Repeatable Deployments

---

# Conclusion

This assignment demonstrates the use of Terraform Infrastructure as Code (IaC) to automate the creation of AWS resources. By provisioning an Amazon S3 Bucket with versioning enabled and securely managing access through IAM Users and Policies, the solution ensures scalability, security, and operational efficiency while following AWS and Terraform best practices.
