# AWS Production-Ready Web Application (EC2 + ALB + ASG)

## Overview
This project demonstrates the design and implementation of a production-grade web application infrastructure on AWS with a strong focus on security, scalability, and cost awareness.

The project is built incrementally, following real-world DevOps practices.

## Phases Completed
- Phase 1: VPC Design and Network Isolation
- Phase 2: IAM Roles and Security Groups
- Phase 3: EC2 Application Server and Golden AMI creation
- Phase 4: Application Load Balancer (ALB) Integration and Validation
- Phase 5: Auto Scaling Group (ASG) and Self-Healing Infrastructure

## Architecture

### Phase 1 – Network Foundation (VPC)
![VPC Architecture](architecture/phase-1-vpc.png)

**Network boundaries**
- Custom VPC (10.0.0.0/16)
- Public subnets for internet-facing components
- Private subnets reserved for application workloads
- Internet Gateway attached only to public route table

---

### Phase 2 – Security Architecture (IAM & Security Groups)
![Security Architecture](architecture/phase-2-iam-security.png)

**Traffic flow and trust lines**
- Internet → ALB (HTTP 80 allowed)
- ALB Security Group → EC2 Security Group (HTTP 80 only)
- ❌ No direct Internet → EC2 access
- ❌ No SSH (Port 22) allowed
> This phase ensures that application instances are not directly accessible and can only be reached through controlled, secure paths.


**Identity flow**
- EC2 instances assume IAM role (`ec2-prod-webapp-role`)
- Permissions granted via IAM policies (SSM, CloudWatch)
- No long-lived access keys

---

### Phase 3 – Compute Layer (EC2 & Golden AMI)
![EC2 AMI Architecture](architecture/phase-3-ec2-ami.png)

**Compute lifecycle**
- Temporary EC2 instance launched for application setup
- Application baked into a golden AMI
- Instance terminated after AMI creation
- AMI reused for future Auto Scaling Groups

---

### Phase 4 – Traffic Management (Application Load Balancer)
![ALB Architecture](architecture/phase-4-alb.png)

**Load balancing behavior**
- Internet-facing ALB deployed in public subnets
- Routes HTTP traffic to EC2 instances in private subnets
- Health checks ensure traffic is sent only to healthy targets
- ALB used temporarily for validation and deleted to control cost

---

### Phase 5 – Self-Healing & Scaling (Auto Scaling Group)
![ASG Architecture](architecture/phase-5-asg.png)

**Scaling and recovery behavior**
- Launch Template created from golden AMI
- Auto Scaling Group deployed across multiple private subnets
- Desired capacity maintained automatically
- Failed instances replaced without manual intervention

---

### End-to-End Flow

```text
User
  |
  v
Internet
  |
  v
[ Application Load Balancer ]
  |
  |  HTTP 80 only (Security Group)
  v
[ EC2 Instances ]
  |
  |  IAM Role (No SSH, No Keys)
  v
AWS Services (SSM, CloudWatch)

```

## AWS Services Used
- VPC
- EC2
- IAM
- Application Load Balancer
- Auto Scaling Group
- CloudWatch (upcoming)

## Cost Awareness
This project is designed with AWS Free Tier constraints in mind while still demonstrating production-grade architecture

- EC2 instances are used temporarily and terminated after AMI creation
- Application Load Balancers are provisioned only for short-term validation and deleted immediately
- Auto Scaling Groups are configured with minimal desired capacity
- NAT Gateways are intentionally avoided unless explicitly required

responsible cloud usage without unnecessary cost.

## Next Steps
- Phase 6: Monitoring and Alerts
