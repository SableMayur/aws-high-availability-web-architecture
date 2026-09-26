# AWS High Availability Web Architecture

## 📌 Project Overview

This project demonstrates a highly available and resilient web application architecture on AWS.

The infrastructure is deployed across **two Availability Zones** using public and private subnets.

The application servers are deployed in private subnets and receive incoming traffic through an **Application Load Balancer (ALB)**.

For outbound internet connectivity, **NAT Gateways** are deployed in both Availability Zones.

An **Auto Scaling Group (ASG)** automatically manages the application servers and maintains availability across both Availability Zones.

---

## 🏗️ Architecture

```text
                         Internet
                            │
                            ▼
                    ┌─────────────────┐
                    │ Application     │
                    │ Load Balancer   │
                    │      (ALB)      │
                    └────────┬────────┘
                             │
              ┌──────────────┴──────────────┐
              │                             │
              ▼                             ▼
       Availability Zone A          Availability Zone B
       ┌─────────────────┐          ┌─────────────────┐
       │ Public Subnet A │          │ Public Subnet B │
       │                 │          │                 │
       │ NAT Gateway A   │          │ NAT Gateway B   │
       │ ALB Node        │          │ ALB Node        │
       └────────┬────────┘          └────────┬────────┘
                │                            │
                ▼                            ▼
       ┌─────────────────┐          ┌─────────────────┐
       │ Private Subnet A│          │ Private Subnet B│
       │                 │          │                 │
       │ EC2 Instance    │          │ EC2 Instance    │
       │                 │          │                 │
       └─────────────────┘          └─────────────────┘
                │                            │
                └────────────┬───────────────┘
                             │
                     Auto Scaling Group
```

---

## ☁️ AWS Services Used

- Amazon VPC
- Amazon EC2
- Application Load Balancer
- Auto Scaling Group
- NAT Gateway
- Internet Gateway
- Route Tables
- Security Groups
- Availability Zones
- Private Subnets
- Public Subnets

---

# 🎯 Project Objectives

The main objectives of this project are:

- Build a highly available AWS network architecture
- Deploy resources across multiple Availability Zones
- Keep application servers inside private subnets
- Distribute incoming traffic using an Application Load Balancer
- Automatically scale application servers
- Provide outbound internet access through NAT Gateways
- Improve fault tolerance using multi-AZ deployment
- Implement network-level security using public and private subnets

---

# 🌐 Network Architecture

## VPC

A dedicated VPC was created for the application infrastructure.

Example:

```text
VPC CIDR: 10.0.0.0/16
```

The VPC contains:

### Public Subnets

```text
Public Subnet A
Public Subnet B
```

The public subnets contain:

- Application Load Balancer nodes
- NAT Gateway A
- NAT Gateway B

### Private Subnets

```text
Private Subnet A
Private Subnet B
```

The EC2 application servers are deployed inside these private subnets.

---

# 🔀 Routing

## Public Route Table

The public route table contains a route to the Internet Gateway.

```text
0.0.0.0/0 → Internet Gateway
```

This allows resources in public subnets to communicate with the internet.

---

## Private Route Tables

Private subnets use NAT Gateways for outbound internet connectivity.

```text
Private Subnet A
       │
       ▼
NAT Gateway A
       │
       ▼
Internet Gateway
       │
       ▼
Internet
```

and:

```text
Private Subnet B
       │
       ▼
NAT Gateway B
       │
       ▼
Internet Gateway
       │
       ▼
Internet
```

---

# ⚖️ Application Load Balancer

An Application Load Balancer was configured across both Availability Zones.

The ALB receives incoming HTTP requests and distributes them to healthy EC2 instances running inside the private subnets.

```text
Client
  │
  ▼
ALB
  │
  ├── EC2 Instance AZ-A
  │
  └── EC2 Instance AZ-B
```

Health checks are used to determine whether the application instances are healthy.

---

# 📈 Auto Scaling Group

An Auto Scaling Group manages the EC2 application servers.

The instances are distributed across two Availability Zones.

Example configuration:

```text
Minimum Capacity: 2
Desired Capacity: 2
Maximum Capacity: 4
```

The Auto Scaling Group can:

- Launch new instances
- Terminate unhealthy instances
- Replace failed instances
- Scale based on configured policies

---

# 🔐 Security

The application servers are deployed in **private subnets**.

They do not receive direct internet traffic.

Incoming application traffic follows:

```text
Internet
   ↓
Application Load Balancer
   ↓
Private EC2 Instances
```

Security Groups are configured so that:

### ALB Security Group

Allows:

```text
HTTP  → 80
HTTPS → 443
```

from the required source.

### EC2 Security Group

Allows application traffic only from the ALB security group.

This prevents direct access to the application servers from the public internet.

---

# 🧪 Testing

The architecture was tested by accessing the application through the Application Load Balancer DNS name.

Example:

```text
http://<ALB-DNS-NAME>
```

The request was successfully forwarded to the EC2 instances running in the private subnets.

---

## High Availability Test

To verify high availability, one application instance can be stopped or terminated.

The Auto Scaling Group detects the unhealthy/missing instance and launches a replacement instance.

The Application Load Balancer continues routing traffic to healthy instances.

---

# 📸 Screenshots

## VPC

![VPC](screenshots/vpc.png)

## Subnets

![Subnets](screenshots/subnets.png)

## NAT Gateways

![NAT Gateways](screenshots/nat-gateways.png)

## Application Load Balancer

![ALB](screenshots/alb.png)

## Target Group

![Target Group](screenshots/target-groups.png)

## Auto Scaling Group

![Auto Scaling](screenshots/auto-scaling.png)

## EC2 Instances

![EC2](screenshots/ec2-private.png)

## Application

![Application](screenshots/working-application.png)

---

# 🧠 What I Learned

Through this project, I gained practical experience with:

- AWS VPC architecture
- Public and private subnet design
- Multi-AZ architecture
- Application Load Balancer
- Auto Scaling Groups
- NAT Gateway
- Internet Gateway
- Route tables
- Security Groups
- EC2
- High availability
- Fault tolerance
- AWS networking

---

# 🚀 Future Improvements

- Recreate the complete infrastructure using Terraform
- Add HTTPS using AWS Certificate Manager
- Configure Route 53
- Implement CI/CD using Jenkins or GitHub Actions
- Add CloudWatch monitoring and alarms
- Implement infrastructure automation
- Store infrastructure code in Terraform modules

---

## 👨‍💻 Author

**Mayur Sable**

Aspiring DevOps & Cloud Engineer

GitHub: [SableMayur](https://github.com/SableMayur)

LinkedIn: [Mayur Sable](https://www.linkedin.com/in/sablemayur/)
