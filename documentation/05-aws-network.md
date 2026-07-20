# 05 - AWS Networking

# Overview

Amazon Web Services (AWS) hosts the production environment for the Enterprise Hybrid Cloud Platform. AWS is responsible for hosting the customer-facing web application, serving as the central WireGuard VPN hub, and providing secure connectivity between Azure and the on-premises security lab.

The AWS environment is designed following enterprise networking best practices by separating public-facing infrastructure from private application and database resources.

---

# AWS Responsibilities

The AWS environment is responsible for:

- Production Web Application
- WireGuard VPN Hub
- Application Routing
- Secure Internet Access
- Cloud Storage
- Monitoring
- Identity & Access Management
- Production Networking

---

# AWS Network Architecture

```
                     Internet
                         │
                  Cloudflare DNS
                         │
                         ▼
                 Internet Gateway
                         │
                         ▼
                 Public Subnet
                 10.0.1.0/24
                         │
      ┌─────────────────────────────────┐
      │                                 │
      │ WireGuard Hub                   │
      │ Application Load Balancer       │
      └─────────────────────────────────┘
                         │
                         ▼
              Private Application Subnet
                    10.0.2.0/24
                         │
      ┌─────────────────────────────────┐
      │                                 │
      │ React Frontend                  │
      │ FastAPI Backend                 │
      └─────────────────────────────────┘
                         │
                         ▼
               Private Database Subnet
                    10.0.3.0/24
                         │
      ┌─────────────────────────────────┐
      │                                 │
      │ PostgreSQL Database             │
      └─────────────────────────────────┘
```

---

# Virtual Private Cloud (VPC)

| Setting | Value |
|----------|-------|
| CIDR Block | 10.0.0.0/16 |
| Region | AWS Deployment Region |
| DNS Resolution | Enabled |
| DNS Hostnames | Enabled |

The VPC provides isolated networking for all AWS resources.

---

# Subnet Design

## Public Subnet

Network

```
10.0.1.0/24
```

Purpose

- Internet-facing resources
- WireGuard VPN Hub
- Application Load Balancer

---

## Private Application Subnet

Network

```
10.0.2.0/24
```

Purpose

- React Frontend
- FastAPI Backend
- Internal application services

This subnet is not directly accessible from the Internet.

---

## Private Database Subnet

Network

```
10.0.3.0/24
```

Purpose

- PostgreSQL Database

Only application servers are permitted to communicate with database resources.

---

# Planned Infrastructure

| Resource | Subnet | Planned IP |
|-----------|--------|------------|
| WireGuard Hub | Public | 10.0.1.40 |
| Application Load Balancer | Public | Dynamic |
| React Application | Application | Dynamic |
| FastAPI Backend | Application | Dynamic |
| PostgreSQL | Database | Dynamic |

---

# Internet Gateway

The Internet Gateway provides Internet connectivity for public-facing AWS resources.

Responsibilities

- Internet Access
- Public Routing
- Load Balancer Connectivity
- WireGuard Endpoint Connectivity

---

# Route Tables

## Public Route Table

Routes

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | Internet Gateway |

---

## WireGuard Routes

The AWS WireGuard Hub advertises:

```
10.0.0.0/16
```

Routes installed on the hub include:

```
10.1.0.0/16 dev wg0

10.2.0.0/16 dev wg0

172.16.100.0/24 dev wg0
```

These routes allow secure communication with Azure and the on-premises security lab.

---

# WireGuard Hub

AWS acts as the central VPN hub.

Tunnel Network

```
172.16.100.1/24
```

Connected Peers

- Azure WireGuard Gateway
- On-Premises WireGuard Gateway

Responsibilities

- VPN Termination
- Traffic Routing
- Packet Forwarding
- Secure Tunnel Management

---

# Security Groups

Security Groups implement instance-level firewall protection.

Example inbound rules:

| Protocol | Port | Purpose |
|----------|------|----------|
| SSH | 22 | Administration |
| HTTPS | 443 | Production Application |
| HTTP | 80 | Redirect to HTTPS |
| UDP | 51820 | WireGuard VPN |

Outbound traffic is permitted as required for application and VPN communication.

---

# Identity and Access Management

AWS IAM controls access to AWS resources.

Responsibilities include:

- Administrative access
- Service roles
- Least privilege permissions
- Secure resource authorization

IAM manages cloud resource permissions and is separate from Active Directory user authentication hosted in Azure.

---

# Cloud Storage

Amazon S3 will be used for:

- Static assets
- Application uploads
- Backup storage
- Deployment artifacts

---

# Monitoring

CloudWatch will provide:

- Metrics
- Logs
- Alerts
- Performance Monitoring

Future enhancements may include centralized dashboards and automated notifications.

---

# Production Application

The production environment will host:

Frontend

- React

Backend

- FastAPI

Database

- PostgreSQL

Application traffic flow:

```
Internet

↓

Cloudflare

↓

Application Load Balancer

↓

React Frontend

↓

FastAPI Backend

↓

PostgreSQL
```

---

# Network Security

Security controls include:

- Security Groups
- WireGuard VPN
- Private Subnets
- Principle of Least Privilege
- IAM Roles
- HTTPS
- Cloudflare Protection

---

# Validation

The following networking components have been validated.

## Completed

- AWS VPC
- Public Subnet
- WireGuard Hub
- AWS ↔ Azure VPN
- AWS ↔ On-Premises VPN
- Linux Routing
- IP Forwarding
- Route Verification

---

# Current Status

## Operational

- AWS Production Network
- WireGuard Hub
- VPN Connectivity
- Static Routing
- Internet Connectivity

## Planned

- Application Load Balancer
- React Frontend
- FastAPI Backend
- PostgreSQL
- CloudWatch
- S3
- HTTPS Deployment

---

# Future Enhancements

- Web Application Firewall (AWS WAF)
- Auto Scaling Groups
- Docker Containers
- Kubernetes (EKS)
- Infrastructure as Code (Terraform)
- Secrets Manager
- AWS Systems Manager
- Multi-AZ Deployment

---

# Summary

AWS serves as the production environment and central networking hub for the Enterprise Hybrid Cloud Platform.

It hosts the customer-facing web application, terminates the WireGuard VPN connections from Azure and the on-premises security lab, manages secure routing between environments, and provides the cloud infrastructure required to support scalable application deployment, monitoring, and future enterprise growth.