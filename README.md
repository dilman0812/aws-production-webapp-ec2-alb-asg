# AWS Production-Ready Web Application (EC2 + ALB + ASG)

## Overview
This project demonstrates the design and implementation of a production-grade web application infrastructure on AWS with a strong focus on security, scalability, and cost awareness.

The project is built incrementally, following real-world DevOps practices.

## Phases Completed
- Phase 1: VPC Design and Network Isolation
- Phase 2: IAM Roles and Security Groups

## Architecture
![Architecture Diagram](architecture/phase-1-vpc.png)

## AWS Services Used
- VPC
- EC2 (upcoming)
- IAM
- Application Load Balancer (upcoming)
- Auto Scaling (upcoming)
- CloudWatch (upcoming)

## Cost Awareness
This project is designed to stay within AWS Free Tier limits. Billable resources such as Load Balancers and NAT Gateways are created only temporarily for validation and then terminated.

## Next Steps
- Phase 3: EC2 Application Server and AMI creation
- Phase 4: Load Balancing and Auto Scaling
- Phase 5: Monitoring and Alerts
