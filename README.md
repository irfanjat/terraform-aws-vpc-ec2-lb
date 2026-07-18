# Terraform AWS VPC EC2 ALB Docker Portfolio Project

This repository contains a Terraform configuration that provisions a simple AWS web stack for hosting a Dockerized portfolio application.

## What this project deploys

- A custom VPC
- Two public subnets across two different Availability Zones
- An Internet Gateway
- A public route table
- Security groups for:
  - the EC2 instance
  - the Application Load Balancer
- An EC2 instance with SSH access and Docker installed
- A Docker container running the image `irfanjat/portolio:latest`
- An Application Load Balancer to distribute traffic to the EC2 instance
- An output showing the EC2 public IP

## Main file

- `aws-vpc-ec2-lb.tf` — Terraform configuration for the AWS infrastructure.

## Current architecture

- EC2 instance is launched in a public subnet
- Docker is installed via EC2 user data
- The portfolio container is started on port 80
- The ALB is attached to the EC2 instance through a target group
- The application is exposed through the ALB and the EC2 public IP

## Prerequisites

- Terraform `>= 1.5.0`
- AWS provider configured with valid credentials
- A public SSH key file available at `~/Downloads/mypair.pub`
- Access to Docker Hub image `irfanjat/portolio:latest`

## Deployment steps

1. Initialize Terraform:

   ```bash
   terraform init
   ```

2. Validate configuration:

   ```bash
   terraform validate
   ```

3. Review infrastructure changes:

   ```bash
   terraform plan
   ```

4. Apply the infrastructure:

   ```bash
   terraform apply -auto-approve
   ```

5. Get the EC2 public IP:

   ```bash
   terraform output public_ip
   ```

6. Check the deployed application:

   ```bash
   curl http://<public-ip>
   ```

## Notes

- The AMI currently uses `ami-0c02fb55956c7d316` for `us-east-1`.
- The EC2 instance type was changed to `t3.micro` to avoid the Free Tier eligibility issue.
- The load balancer requires two subnets in two different AZs, so the configuration now includes both `us-east-1a` and `us-east-1b` subnets.
- The Docker bootstrap is handled through `user_data` in the EC2 resource.

## Cleanup

To remove everything created in AWS:

```bash
terraform destroy
```
