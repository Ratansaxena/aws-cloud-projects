# Assignment 03 - Nginx Reverse Proxy, High Availability, Disaster Recovery & Auto Scaling on AWS

## Problem Statement

A client is experiencing performance and scalability issues in their AWS-hosted application environment due to increasing traffic. To improve request handling and reduce backend load, the client wants to introduce Nginx as a reverse proxy layer between users and application services.

In addition to traffic management, the client requires High Availability (HA), Disaster Recovery (DR), version-controlled deployments, rollback capabilities, auto-scaling, secure access management, content hosting through Nginx, S3 integration, CloudFront CDN implementation, and path-based routing through a single Application Load Balancer.

The solution must be implemented entirely within AWS while following best practices for scalability, security, reliability, and automation.

---

# Objective

The objective of this assignment is to design and implement a highly available, scalable, and secure AWS infrastructure using Nginx as a reverse proxy while demonstrating:

* High Availability (HA)
* Disaster Recovery (DR)
* Auto Scaling
* Load Balancing
* Version Management using AMIs
* Rolling Deployment Strategy
* Rollback Mechanism
* S3 Integration
* CloudFront CDN Integration
* Path-Based Routing
* Secure IAM Access Management

---

# Architecture Diagram

```text
                                  INTERNET
                                      │
                                      ▼
                           ┌───────────────────┐
                           │   CloudFront CDN │
                           └───────────────────┘
                                      │
                                      ▼
                          ┌──────────────────────┐
                          │ Application Load     │
                          │ Balancer (ALB)       │
                          └──────────────────────┘
                                      │
                  ┌───────────────────┴───────────────────┐
                  │                                       │
                  ▼                                       ▼
         Target Group 1                          Target Group 2
           (/ninja1)                               (/ninja2)
                  │                                       │
                  ▼                                       ▼
          Nginx Server-1                         Nginx Server-2
          Private Subnet-A                       Private Subnet-B
                  │                                       │
                  └───────────────┬───────────────────────┘
                                  │
                                  ▼
                        Auto Scaling Group
                                  │
                                  ▼
                           Launch Template
                                  │
                                  ▼
                             AMI V1 / V2

──────────────────────────────────────────────────────────

VPC (10.0.0.0/16)

├── Public Subnet-A
│   ├── Bastion Host
│   ├── ALB
│   └── NAT Gateway
│
├── Public Subnet-B
│   ├── ALB
│   └── NAT Gateway
│
├── Private Subnet-A
│   └── Nginx Server-1
│
└── Private Subnet-B
    └── Nginx Server-2

S3 Bucket
├── prod/
└── nonprod/
```

---

# Day 1 - Nginx Versioning & Auto Scaling

## Tasks Performed

* Created EC2 instance.
* Installed Nginx.
* Created AMI Version 1 (AMI-V1).
* Launched new EC2 instance using AMI-V1.
* Modified Nginx configuration.
* Created AMI Version 2 (AMI-V2).
* Launched EC2 instance using AMI-V2.
* Configured Launch Template.
* Configured Auto Scaling Group.
* Attached Auto Scaling Group to Application Load Balancer.
* Performed Load Testing.
* Verified Auto Scaling behavior.

## Scaling Policies Tested

### Average CPU Utilization

* Scale Out > 70%
* Scale In < 30%

### Network Bytes In/Out

* Monitored network traffic growth.

### ALB Request Count Per Target

* Verified scaling based on request volume.

## Upgrade Strategy

AMI-V1 → AMI-V2

## Rollback Strategy

AMI-V2 → AMI-V1

---

# Day 2 - Static Website Hosting

## Tasks Performed

* Pulled image content from Git repository.
* Uploaded images to Amazon S3 using AWS CLI.
* Used IAM Role instead of Access Keys.
* Configured Nginx frontend page.
* Served images directly from S3 Bucket.

## Benefits

* Secure credential management.
* Centralized image storage.
* Reduced server storage requirements.

---

# Day 3 - Auto Scaling Health Check Validation

## Tasks Performed

* Intentionally made Nginx instance unhealthy.
* Observed Auto Scaling Group behavior.
* Verified automatic instance replacement.
* Ensured Desired Capacity remained unchanged.

## Result

ASG automatically terminated unhealthy instance and launched a replacement instance.

---

# Day 4 - Path Based Routing

## Infrastructure

### Bastion Host

* Located in Public Subnet.
* SSH access allowed only from public IP.

### Nginx Servers

* One server in each private subnet.
* SSH access allowed only from Bastion Host.

### ALB Listener Rules

#### Path 1

```text
/ ninja1
```

Displays:

```text
Image-1
```

#### Path 2

```text
/ ninja2
```

Displays:

```text
Image-2
```

### Security Controls

* ALB accessible only from public IP.
* Nginx accessible only through ALB.
* Private instances inaccessible from internet.

---

# Day 5 - IAM & S3 Restrictions

## S3 Bucket Structure

```text
bucket-name
│
├── prod/
│
└── nonprod/
```

## Tasks Performed

* Created IAM User.
* Created IAM Role.
* Created S3 Bucket.
* Uploaded separate images to both folders.

## Access Restrictions

### Root User

Full Access

### IAM User

Access:

* nonprod/

Restricted:

* prod/

### Nginx Application

Access through IAM Role.

---

# Day 6 - CloudFront & IAM Trust Relationships

## Tasks Performed

* Configured CloudFront Distribution.
* Integrated CloudFront with S3.
* Configured Trust Relationship.
* Reused IAM Role through AssumeRole mechanism.
* Implemented Least Privilege Access.

## Benefits

* Reduced latency.
* Global content delivery.
* Improved performance.

---

# AWS Services Used

* Amazon EC2
* Amazon AMI
* Amazon VPC
* Public Subnets
* Private Subnets
* Internet Gateway
* NAT Gateway
* Application Load Balancer
* Auto Scaling Group
* Launch Template
* Amazon S3
* Amazon CloudFront
* AWS IAM
* Security Groups
* CloudWatch

---

# High Availability Strategy

* Multi-AZ Deployment
* Auto Scaling Group
* Load Balancer Health Checks
* Instance Self-Healing
* Redundant Nginx Servers

---

# Disaster Recovery Strategy

* Versioned AMIs
* Launch Templates
* Rollback Mechanism
* Auto Instance Recovery
* Cloud-Based Static Content Storage

---

# Blue-Green Deployment Strategy

Blue Environment

* AMI V1

Green Environment

* AMI V2

Deployment Flow:

1. Deploy V2.
2. Validate application.
3. Shift traffic gradually.
4. Rollback instantly if issue occurs.

---

# Security Best Practices

* Bastion Host Access
* Least Privilege IAM
* IAM Roles instead of Access Keys
* Private Application Servers
* Restricted Security Groups
* Path-Based Routing Security
* S3 Bucket Policies

---

# Expected Outcome

* Highly Available Infrastructure
* Automatic Scaling
* Version Controlled Deployments
* Secure Access Management
* Path-Based Routing
* CloudFront Accelerated Content Delivery
* Rollback Support
* Self-Healing Infrastructure

---

# Conclusion

This project successfully demonstrates the implementation of a production-grade AWS infrastructure using Nginx as a reverse proxy with High Availability, Disaster Recovery, Auto Scaling, Blue-Green Deployment, CloudFront CDN, S3 Integration, IAM Security Controls, and Path-Based Routing. The solution ensures scalability, fault tolerance, security, and operational excellence while following AWS best practices.
