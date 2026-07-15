# 05 - AWS Network

# Enterprise Hybrid Cloud Platform - AWS Network Design

## Overview

Amazon Web Services (AWS) serves as the primary application hosting environment for the Enterprise Hybrid Cloud Platform. It provides the public-facing infrastructure responsible for delivering the web application while also acting as the central networking hub for secure hybrid cloud connectivity.

The AWS environment is designed using a segmented Virtual Private Cloud (VPC) with dedicated public and private subnets to separate Internet-facing resources, application workloads, and database services.

---

# Design Objectives

The AWS environment has been designed to:

- Host the production web application
- Provide secure Internet access through Cloudflare
- Separate workloads using network segmentation
- Support containerized application deployment
- Protect backend services from direct Internet exposure
- Securely connect AWS to Azure and the on-premises environment
- Support Infrastructure as Code (Terraform)
- Provide a scalable cloud foundation

---

# AWS Virtual Private Cloud (VPC)

All AWS resources are deployed inside a dedicated Amazon Virtual Private Cloud.

## VPC Configuration

| Property | Value |
|----------|-------|
| Network | AWS VPC |
| CIDR Block | 10.0.0.0/16 |

Using a /16 network allows additional subnets to be created without redesigning the network.

---

# Subnet Design

The VPC is divided into three primary subnets.

## Public Subnet

| CIDR | 10.0.1.0/24 |

Purpose:

- Internet-facing infrastructure
- Public entry point into AWS

Resources:

- Internet Gateway
- Application Load Balancer
- Ubuntu EC2 (WireGuard VPN Gateway)

Only resources requiring public connectivity are deployed within this subnet.

---

## Private Application Subnet

| CIDR | 10.0.2.0/24 |

Purpose:

- Host application workloads
- Prevent direct Internet access
- Isolate business logic

Resources:

- Amazon ECS Cluster
- React Frontend
- FastAPI Backend

Application traffic reaches this subnet only through the Application Load Balancer.

---

## Private Database Subnet

| CIDR | 10.0.3.0/24 |

Purpose:

- Securely store application data
- Prevent public access

Resources:

- Amazon RDS PostgreSQL

Only approved application services are permitted to communicate with the database.

---

# Internet Connectivity

Public client traffic enters AWS using the following path:

```
Internet
      │
Cloudflare
      │
Internet Gateway
      │
Application Load Balancer
      │
Amazon ECS
      │
Amazon RDS
```

Only the Application Load Balancer is exposed to client traffic.

---

# Application Load Balancer

The Application Load Balancer (ALB) distributes incoming HTTPS traffic to application services.

Responsibilities include:

- HTTPS termination
- Layer 7 load balancing
- Health checks
- Request routing
- Future horizontal scaling

The ALB acts as the public entry point for application traffic.

---

# Amazon ECS

Amazon Elastic Container Service (ECS) hosts the application.

Planned services include:

- React Frontend
- FastAPI Backend

Benefits include:

- Container orchestration
- Simplified deployments
- Independent application scaling
- High availability
- Efficient resource utilization

ECS is responsible only for running the application and does not participate in VPN routing.

---

# Amazon RDS

Amazon RDS provides a managed PostgreSQL database.

Benefits include:

- Automated backups
- High availability
- Managed patching
- Monitoring
- Recovery

The database remains isolated inside a dedicated private subnet.

---

# VPN Gateway

AWS serves as the VPN hub for the hybrid cloud platform.

A dedicated Ubuntu EC2 instance running WireGuard functions as the AWS VPN Gateway.

Responsibilities include:

- Encrypting VPN traffic
- Decrypting VPN traffic
- Routing traffic between AWS, Azure, and the on-premises environment
- Providing secure hybrid cloud connectivity

The VPN gateway is deployed separately from the application infrastructure to isolate networking services from application workloads.

---

# Hybrid Cloud Connectivity

AWS maintains encrypted WireGuard tunnels with:

- Microsoft Azure
- On-Premises Network

Using a hub-and-spoke topology, AWS acts as the central routing point for hybrid cloud communication.

Traffic between Azure and the on-premises environment traverses AWS through the VPN hub.

Detailed VPN implementation is documented in **07-vpn-architecture.md**.

---

# Routing

Traffic routing within AWS occurs in multiple stages.

1. AWS Route Tables determine where packets should be forwarded.
2. The WireGuard VPN Gateway processes hybrid cloud traffic.
3. Linux routing forwards traffic through the appropriate interface.
4. WireGuard encrypts packets and forwards them to the correct VPN peer.

This layered routing model separates cloud networking from application processing.

---

# Security Model

The AWS environment follows a defense-in-depth security model.

Security is achieved through:

- Public and private subnet separation
- Least privilege access
- Dedicated VPN gateway
- Application isolation
- Database isolation
- Secure hybrid connectivity

Future security enhancements include:

- Security Groups
- Network ACLs
- AWS WAF
- AWS Secrets Manager
- IAM Roles

---

# Monitoring

Operational visibility will be provided through Amazon CloudWatch.

Monitoring will include:

- ECS performance
- EC2 performance
- VPN health
- Application logs
- Database performance
- Load Balancer metrics
- Infrastructure metrics

---

# Future Improvements

Future enhancements include:

- Multi-Availability Zone deployment
- Auto Scaling
- NAT Gateway
- Additional Route Tables
- CloudWatch dashboards
- GitHub Actions CI/CD
- Terraform deployment
- AWS Systems Manager

---

# Design Decisions

### Why a /16 VPC?

Provides sufficient address space for future growth while simplifying subnet allocation.

### Why separate subnets?

Separating Internet-facing resources, application services, and databases improves security and simplifies administration.

### Why ECS?

ECS provides a scalable platform for containerized applications while separating infrastructure from application logic.

### Why a dedicated WireGuard EC2?

WireGuard is deployed on a dedicated Ubuntu EC2 instance because AWS does not provide a managed WireGuard service. Separating the VPN gateway from ECS ensures networking functions remain independent from application workloads.

### Why AWS as the VPN hub?

AWS hosts the production application and already serves as the primary entry point for Internet traffic. Using AWS as the VPN hub simplifies routing, reduces VPN complexity, and provides a scalable foundation for future expansion.

---

# Summary

AWS serves as both the production application platform and the central networking hub for the Enterprise Hybrid Cloud Platform. By combining a segmented VPC, Amazon ECS, Amazon RDS, an Application Load Balancer, and a dedicated WireGuard VPN Gateway, the AWS environment provides a secure, scalable, and enterprise-ready foundation for hybrid cloud application hosting.