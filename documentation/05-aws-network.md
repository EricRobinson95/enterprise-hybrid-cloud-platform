# 05 - AWS Network

# Enterprise Hybrid Cloud Platform - AWS Network Design

## Overview

Amazon Web Services (AWS) provides the public cloud infrastructure for the Enterprise Hybrid Cloud Platform. The AWS environment is responsible for hosting the production web application while following enterprise networking, security, and cloud architecture best practices.

The AWS environment has been designed using a layered network architecture that separates Internet-facing resources, application services, and database services into dedicated network segments.

---

# Design Objectives

The AWS environment has been designed with the following objectives:

- Host the public web application
- Protect private resources from direct Internet access
- Separate workloads into dedicated network tiers
- Support containerized applications
- Provide a scalable cloud platform
- Minimize the attack surface
- Integrate with Azure and the on-premises environment
- Support future Infrastructure as Code deployments

---

# Virtual Private Cloud (VPC)

All AWS resources are deployed inside a dedicated Amazon Virtual Private Cloud (VPC).

## VPC Configuration

| Property | Value |
|----------|-------|
| CIDR Block | 10.0.0.0/16 |

The VPC acts as the private network boundary for the AWS environment.

Using a /16 network allows the platform to create additional subnets in the future without requiring network redesign.

---

# Subnet Design

The VPC is divided into three security zones.

## Public Subnet

| CIDR | 10.0.1.0/24 |

Purpose:

- Accept Internet traffic
- Host Internet-facing infrastructure

Resources:

- Application Load Balancer

Only resources that must receive public traffic are placed in this subnet.

---

## Private Application Subnet

| CIDR | 10.0.2.0/24 |

Purpose:

- Run containerized application services
- Prevent direct Internet access
- Isolate application processing

Resources:

- Amazon ECS Cluster

The application subnet hosts the business logic for the platform.

Future ECS services include:

- React Frontend
- FastAPI Backend

---

## Private Database Subnet

| CIDR | 10.0.3.0/24 |

Purpose:

- Store application data
- Prevent public access
- Isolate database services

Resources:

- Amazon RDS PostgreSQL

The database subnet is accessible only by approved application services.

---

# Internet Gateway

The Internet Gateway connects the VPC to the public Internet.

Responsibilities include:

- Allow inbound HTTPS traffic
- Allow outbound Internet connectivity
- Serve as the public entry point into the VPC

The Internet Gateway does not perform routing decisions or load balancing.

---

# Application Load Balancer

The Application Load Balancer (ALB) receives incoming application requests.

Responsibilities include:

- Accept HTTPS traffic
- Perform Layer 7 routing
- Forward requests to the appropriate application service
- Support future horizontal scaling
- Perform health checks

The ALB is the only application component exposed to public traffic.

---

# Amazon ECS

Amazon Elastic Container Service (ECS) provides the container platform for the application.

Benefits include:

- Container orchestration
- Simplified deployments
- Independent service updates
- Horizontal scaling
- Improved resource utilization

The ECS cluster will host multiple application services.

Planned services include:

- React Frontend
- FastAPI Backend

Future services can be deployed without modifying the underlying network architecture.

---

# Amazon RDS PostgreSQL

Amazon RDS provides a managed PostgreSQL database.

Advantages include:

- Automated backups
- High availability
- Managed patching
- Monitoring
- Automated recovery

Keeping the database inside a dedicated private subnet reduces the overall attack surface.

---

# Security Model

The AWS environment follows a defense-in-depth strategy.

Security is achieved through:

- Network segmentation
- Private subnets
- Least privilege access
- Layered application architecture
- Container isolation
- Database isolation

Future security services include:

- Security Groups
- Network ACLs
- AWS Secrets Manager
- IAM Roles
- AWS WAF

---

# Monitoring

Operational visibility will be provided through Amazon CloudWatch.

Monitoring will include:

- ECS performance
- Container health
- Application logs
- Database performance
- Load Balancer metrics
- Infrastructure metrics

---

# High Availability

The network has been designed for future expansion.

Planned improvements include:

- Multi-Availability Zone deployment
- Auto Scaling
- NAT Gateway
- Route Tables
- Additional private subnets
- ECS service scaling

These enhancements can be added without redesigning the existing network.

---

# Design Decisions

Several architectural decisions were made during the design of the AWS environment.

### Why a /16 VPC?

A /16 network provides flexibility for future growth while allowing multiple /24 subnets to be created as additional services are deployed.

### Why separate subnets?

Separating public, application, and database resources limits unnecessary communication and improves security.

### Why ECS?

Containerization provides a modern deployment platform that supports independent application services and future scalability.

### Why Amazon RDS?

Using a managed database service reduces operational overhead while improving reliability and backup capabilities.

---

# Future Improvements

The AWS environment will continue to evolve throughout the project.

Planned additions include:

- NAT Gateway
- Route Tables
- Security Groups
- Multi-AZ deployment
- ECS Services
- ECS Tasks
- GitHub Actions CI/CD
- Terraform deployment
- CloudWatch dashboards
- AWS Systems Manager

---

# Summary

The AWS environment provides a secure, scalable, and production-oriented foundation for hosting the Enterprise Hybrid Cloud Platform. By combining a segmented VPC, Amazon ECS, Amazon RDS, and an Application Load Balancer, the design follows modern cloud architecture principles while remaining flexible enough to support future growth and additional enterprise services.