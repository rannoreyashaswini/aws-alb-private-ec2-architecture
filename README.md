# aws-alb-private-ec2-architecture
Demo setup of Application Load Balancer (ALB) in public subnet to route traffic securely to EC2 targets hosted private subnets.


## Overview
This repo demonstrates a common AWS architecture pattern:
- Application Load Balancer (ALB) in a public subnet
- EC2 instances in private subnets as target group members

## Architecture Diagram
Please refer architecture diagram

## Setup Steps
Step-by-Step Setup Guide

1. Create a VPC
CIDR block: 10.0.0.0/16
Enable DNS hostnames and DNS support.

2. Create Subnets
Public Subnets (for ALB)
Example: 10.0.1.0/24
Enable auto-assign public IP.

Private Subnets (for EC2)
Example: 10.0.2.0/24 and 10.0.3.0/24
No public IP assignment.

3. Create an Internet Gateway
Attach it to your VPC.
Update the public route table:
Destination: 0.0.0.0/0
Target: Internet Gateway.

4. Create a NAT Gateway
Place it in a public subnet.
Update the private route table:
Destination: 0.0.0.0/0
Target: NAT Gateway (for outbound traffic).

5. Launch EC2 Instances
Launch instances in private subnets.
Assign a security group that allows inbound traffic only from the ALB security group.

6. Create an Application Load Balancer
Place it in public subnets.
Assign a security group that allows inbound HTTP/HTTPS from the internet.
Configure listeners (port 80 or 443).

7. Create Target Groups
Target type: EC2 instances.
Health check path: / or your app endpoint.
Register private EC2 instances.

8. Update Security Groups
ALB SG: Allow inbound from 0.0.0.0/0 (HTTP/HTTPS).
EC2 SG: Allow inbound from ALB SG only.

9. Test Connectivity
Deploy a simple web app on the EC2 instances in private subnets.

Install Apache or Nginx on the EC2 instance:
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd

Create a simple index page:
echo "<h1>Hello from Private EC2 via ALB</h1>" | sudo tee /var/www/html/index.html

Access the ALB DNS name from your browser.
Verify that traffic reaches EC2 instances in private subnets.

===========================================================================================================================

Troubleshooting
Health check failures: Ensure EC2 SG allows traffic from ALB SG.
No internet access: Use NAT Gateway for outbound traffic from private EC2s.

Cost Considerations  
NAT Gateways, ALBs, and EC2 instances incur costs, so should clean up resources after testing.

Takeaways
ALB in public subnet handles external traffic.
EC2s in private subnets remain secure and isolated.
NAT Gateway enables outbound internet access for private instances.
This architecture is ideal for secure web applications and backend services.
