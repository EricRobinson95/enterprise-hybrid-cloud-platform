# AWS Network Architecture

The AWS environment hosts the public-facing application and backend services. The environment is deployed within a dedicated Virtual Private Cloud (VPC) using the 10.0.0.0/16 private address space.

## VPC

- CIDR Block: 10.0.0.0/16
- Internet Gateway attached to the VPC
- Application Load Balancer in the public subnet
- Amazon ECS Cluster in the private application subnet
- Amazon RDS PostgreSQL in the private database subnet

## Subnets

| Subnet | CIDR | Purpose |
|---------|------|---------|
| Public Subnet | 10.0.1.0/24 | Application Load Balancer |
| Private Application Subnet | 10.0.2.0/24 | Amazon ECS Cluster |
| Private Database Subnet | 10.0.3.0/24 | Amazon RDS PostgreSQL |

## Traffic Flow

Internet
→ Cloudflare
→ Internet Gateway
→ Application Load Balancer
→ Amazon ECS
→ Amazon RDS PostgreSQL