# aws-alb-private-ec2-architecture
Demo setup of Application Load Balancer (ALB) in public subnet to route traffic securely to EC2 targets hosted private subnets.


## Overview
This repo demonstrates a common AWS architecture pattern:
- Application Load Balancer (ALB) in a public subnet
- EC2 instances in private subnets as target group members

## Architecture Diagram
![Architecture Diagram](diagram.png)

## Setup Steps
1. Create a VPC with public and private subnets.
2. Launch EC2 instances in private subnets.
3. Create an ALB in the public subnet.
4. Register EC2 instances in the target group.
5. Configure security groups:
   - ALB SG: allow inbound HTTP/HTTPS from internet.
   - EC2 SG: allow inbound only from ALB SG.
6. Test connectivity.
