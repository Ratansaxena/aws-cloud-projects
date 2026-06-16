# AWS Assignment 01: Deploying Spring3Hibernate Application using Load Balancer & Auto Scaling Group

## Project Overview

This project demonstrates the design and implementation of a highly available, scalable, and secure AWS infrastructure for deploying a Spring3Hibernate web application.

The application is hosted on EC2 instances running in private subnets and is accessed through an Application Load Balancer (ALB). Auto Scaling Group (ASG) ensures automatic scaling and fault tolerance based on traffic demand.

---

## Assignment Objective

The goal of this assignment is to design and implement a cloud infrastructure that supports the deployment of a Spring 3 Hibernate application.

The infrastructure should provide:

* High Availability
* Scalability
* Security
* Fault Tolerance
* Private Application Hosting

The application servers are deployed in private subnets while the Load Balancer handles public traffic.

---

## Spring3Hibernate Application

Repository:

https://github.com/opstree/spring3hibernate

### Application Features

* Employee CRUD Operations
* Add Employee
* Update Employee
* Delete Employee
* Employee Listing
* File Upload
* Image Gallery

### Technology Stack

* Java
* Spring MVC
* Hibernate
* Maven
* Apache Tomcat
* MySQL
* AWS EC2

---

# AWS Architecture

## Infrastructure Components

### VPC (Virtual Private Cloud)

Provides network isolation for all AWS resources.

### Public Subnets

Contains:

* Application Load Balancer (ALB)
* NAT Gateway

### Private Subnets

Contains:

* EC2 Application Servers
* Auto Scaling Group Instances

### Application Load Balancer

Responsible for:

* Traffic Distribution
* Health Checks
* High Availability

### Auto Scaling Group

Responsible for:

* Automatic Scaling
* Instance Recovery
* Fault Tolerance

### NAT Gateway

Allows private instances to access the internet for updates and package installation.

### Security Groups

Configured to allow only required traffic between resources.

---

# Infrastructure Flow Diagram

```text
                         INTERNET
                             │
                             ▼
                  ┌───────────────────┐
                  │ Application Load  │
                  │     Balancer      │
                  │  (Public Subnet)  │
                  └─────────┬─────────┘
                            │
                            ▼
                    ALB Target Group
                            │
            ┌───────────────┴───────────────┐
            │                               │
            ▼                               ▼
    EC2 Instance 1                  EC2 Instance 2
   (Private Subnet)                (Private Subnet)
            │                               │
            └───────────────┬───────────────┘
                            ▼
                  Auto Scaling Group
                            │
                            ▼
                     MySQL Database
                            │
                            ▼
                      NAT Gateway
                            │
                            ▼
                         Internet
```

---

## Network Flow

1. User sends request from browser.
2. Request reaches Application Load Balancer.
3. ALB forwards request to healthy EC2 instances.
4. EC2 instances process application requests.
5. Application communicates with MySQL database.
6. Auto Scaling Group monitors instance health.
7. New instances are launched automatically when required.
8. NAT Gateway provides outbound internet connectivity.

---

## Security Architecture

### Load Balancer Security Group

Inbound:

* HTTP (80)
* HTTPS (443)

Outbound:

* Application Servers

### EC2 Security Group

Inbound:

* Traffic only from ALB

Outbound:

* NAT Gateway

Benefits:

* No direct internet access to application servers.
* Improved security posture.
* Reduced attack surface.

---

## Auto Scaling Configuration

| Setting          | Value |
| ---------------- | ----- |
| Minimum Capacity | 2     |
| Desired Capacity | 2     |
| Maximum Capacity | 4     |

### Scaling Policy

Scale Out:

* CPU Utilization > 70%

Scale In:

* CPU Utilization < 30%

---

## Deployment Steps

### Clone Repository

```bash
git clone https://github.com/opstree/spring3hibernate.git
cd spring3hibernate
```

### Build Application

```bash
mvn clean package
```

### Deploy WAR File

```bash
cp target/Spring3HibernateApp.war /opt/tomcat/webapps/
```

### Restart Tomcat

```bash
sudo systemctl restart tomcat
```

### Access Application

```text
http://<ALB-DNS-NAME>/Spring3HibernateApp/
```

---

## AWS Services Used

* Amazon EC2
* Amazon VPC
* Application Load Balancer
* Auto Scaling Group
* Security Groups
* NAT Gateway
* Amazon CloudWatch
* IAM

---

## Project Structure

```text
spring3hibernate/
│
├── README.md
├── architecture-diagram/
│   └── aws-architecture.png
│
├── screenshots/
│   ├── vpc.png
│   ├── alb.png
│   ├── asg.png
│   ├── ec2.png
│   ├── target-group.png
│   └── application-homepage.png
│
├── src/
└── pom.xml
```

---

## Deliverables

* Infrastructure Design Diagram
* VPC Configuration
* Public & Private Subnets
* Load Balancer Configuration
* Auto Scaling Group Configuration
* Security Group Configuration
* NAT Gateway Configuration
* Spring3Hibernate Deployment
* Implementation Documentation

---

## Learning Outcomes

This assignment helped in understanding:

* AWS Networking
* VPC Design
* Load Balancer Configuration
* Auto Scaling Implementation
* EC2 Management
* Security Best Practices
* Spring MVC Deployment
* Apache Tomcat Administration

---

## Author

**Ratnu**

AWS DevOps Assignment 01

---

## License

This project is created for educational and learning purposes.
