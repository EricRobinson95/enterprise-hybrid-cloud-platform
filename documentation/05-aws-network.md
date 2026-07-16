# 05 - AWS Network

## Overview

The AWS environment hosts the public-facing infrastructure for the Enterprise Hybrid Cloud Platform. It provides secure connectivity to the Internet, hosts the WireGuard VPN gateway, and serves as the cloud platform for future application workloads.

The AWS network is designed using enterprise networking best practices, including network segmentation, least privilege, dedicated routing, and secure communication with Microsoft Azure through a site-to-site WireGuard VPN.

---

# AWS Region

| Setting | Value |
|----------|-------|
| Region | US East (Ohio) (us-east-2) |
| Availability Zone | us-east-2a |

---

# VPC Configuration

| Setting | Value |
|----------|-------|
| Name | production-aws-vpc |
| CIDR Block | 10.0.0.0/16 |
| DNS Resolution | Enabled |
| DNS Hostnames | Enabled |
| IPv6 | Disabled |

### Why /16?

The VPC uses a **/16** CIDR block to provide a large address space for future growth.

Benefits include:

- Support for many additional subnets
- Easier network segmentation
- Room for production, development, testing, and disaster recovery environments
- Simplified future expansion without redesigning the network

---

# Subnet Design

## Public Subnet

| Setting | Value |
|----------|-------|
| Name | public-subnet |
| CIDR | 10.0.1.0/24 |
| Purpose | Internet-facing resources |

Hosts:

- WireGuard VPN Gateway
- Future Application Load Balancer (ALB)

Characteristics:

- Associated with Public Route Table
- Internet access through Internet Gateway
- Public IP addresses enabled when required

---

## Private Application Subnet

| Setting | Value |
|----------|-------|
| Name | private-application-subnet |
| CIDR | 10.0.2.0/24 |
| Purpose | Application servers |

Characteristics:

- No direct Internet access
- Receives traffic only from trusted resources
- Future application servers and containers

---

## Private Database Subnet

| Setting | Value |
|----------|-------|
| Name | private-database-subnet |
| CIDR | 10.0.3.0/24 |
| Purpose | Database tier |

Characteristics:

- Completely isolated from the Internet
- Accessible only by approved application servers
- Future Amazon RDS deployment

---

# Internet Gateway

## Name

```
production-internet-gateway
```

Purpose:

Provides Internet connectivity between the AWS VPC and the public Internet.

The Internet Gateway is attached directly to the production VPC and is used by the public route table to provide outbound and inbound Internet communication.

---

# Route Tables

## Public Route Table

Name

```
production-public-route-table
```

Routes

| Destination | Target |
|------------|---------|
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | Internet Gateway |

Associated Subnets

- public-subnet

Purpose

Allows Internet-bound traffic from the public subnet to leave the VPC through the Internet Gateway while maintaining local communication inside the VPC.

---

## Default Route Table

The default route table remains in the VPC but is not used for production workloads.

All production resources use dedicated route tables to improve security, organization, and scalability.

---

# EC2 WireGuard Gateway

## Instance Information

| Setting | Value |
|----------|-------|
| Name | production-wireguard-gateway |
| Operating System | Ubuntu Server 26.04 LTS |
| Architecture | x86 (amd64) |
| Instance Type | t3.micro |
| Storage | 8 GiB gp3 SSD |
| Placement | public-subnet |

Purpose

This EC2 instance serves as the secure VPN gateway between AWS and Microsoft Azure.

Responsibilities include:

- Hosting the WireGuard VPN service
- Encrypting and decrypting VPN traffic
- Routing traffic between AWS and Azure
- Acting as the secure entry point into the AWS environment

---

# Network Interface (ENI)

Every EC2 instance connects to the VPC through an Elastic Network Interface (ENI).

The ENI is responsible for:

- Private IP addressing
- MAC address
- Security Group association
- Public IP association
- Communication with the subnet

Understanding the ENI is important because security groups are applied to the network interface rather than directly to the EC2 instance.

---

# Security Group

## Name

```
production-wireguard-sg
```

### Inbound Rules

| Protocol | Port | Source | Purpose |
|----------|------|--------|---------|
| TCP | 22 | Administrator Public IP (/32) | SSH Administration |
| UDP | 51820 | 0.0.0.0/0 | WireGuard VPN Clients |

### Outbound Rules

| Protocol | Destination | Purpose |
|----------|-------------|---------|
| All Traffic | 0.0.0.0/0 | Outbound communication |

The security group follows the Principle of Least Privilege by restricting administrative SSH access to the administrator's public IP while allowing WireGuard VPN traffic from authorized clients.

---

# Public IP Addressing

The WireGuard gateway receives:

- Private IPv4 Address (10.0.1.0/24)
- Public IPv4 Address

The public IP allows:

- SSH administration
- WireGuard VPN connectivity

Future enhancement:

The temporary public IP will be replaced with an Elastic IP to provide a permanent VPN endpoint.

---

# Traffic Flow

## Administrative SSH Access

```
Administrator Laptop
        │
        ▼
Public Internet
        │
        ▼
Internet Gateway
        │
        ▼
Public Route Table
        │
        ▼
Public Subnet
        │
        ▼
Security Group
        │
        ▼
production-wireguard-gateway
```

---

## WireGuard VPN Traffic

```
Azure Virtual Network
        │
Encrypted WireGuard Tunnel
        │
        ▼
Public Internet
        │
        ▼
Internet Gateway
        │
        ▼
production-wireguard-gateway
        │
Decrypt
        │
Routing
        │
Forward to AWS Resources
```

---

# Current AWS Infrastructure

Completed

- Production VPC
- Public Subnet
- Private Application Subnet
- Private Database Subnet
- Internet Gateway
- Public Route Table
- Public Route Association
- Ubuntu EC2 Instance
- WireGuard Security Group
- SSH Key Pair
- IAM Administrator Account
- MFA Enabled

In Progress

- WireGuard Installation
- Azure VPN Gateway Configuration
- Static Route Configuration
- Cloudflare Integration

Planned

- NAT Gateway
- Elastic IP
- Application Load Balancer
- Auto Scaling
- ECS Containers
- Amazon RDS
- CloudWatch Monitoring
- AWS Backup

---

# Design Decisions

- Dedicated VPC for complete network isolation.
- /16 VPC provides long-term scalability.
- Separate public and private subnets improve security.
- Dedicated public route table simplifies routing management.
- Ubuntu Server LTS selected for long-term stability.
- t3.micro selected because the WireGuard gateway has low CPU and memory requirements.
- SSH access restricted to the administrator's public IP.
- WireGuard uses UDP port 51820.
- Security groups implement stateful firewall protection.
- Public resources are isolated from future private application and database workloads.
- Hybrid cloud communication is secured using WireGuard site-to-site VPN.

---

# Future Improvements

- Replace temporary public IP with an Elastic IP.
- Deploy NAT Gateway for private subnet Internet access.
- Create private route tables.
- Deploy application servers in the private subnet.
- Deploy Amazon RDS in the database subnet.
- Implement CloudWatch monitoring and alarms.
- Manage infrastructure using Terraform.
- Automate deployments through Infrastructure as Code.