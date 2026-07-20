# 03 - Network Architecture

# Overview

The Enterprise Hybrid Cloud Platform is a hybrid cloud environment designed to simulate a real-world enterprise infrastructure spanning Amazon Web Services (AWS), Microsoft Azure, and an on-premises security lab.

The architecture combines cloud infrastructure, enterprise identity management, production application hosting, secure networking, and cybersecurity into a single environment.

Each environment has a dedicated responsibility:

- **AWS** hosts the production web application and serves as the WireGuard VPN hub.
- **Azure** provides enterprise identity services and the managed developer environment.
- **On-Premises** functions as the organization's security operations and penetration testing lab.

---

# Architecture Goals

- Design a scalable hybrid cloud infrastructure
- Separate production, corporate IT, and security environments
- Secure inter-site communication using WireGuard
- Host production workloads in AWS
- Centralize enterprise identity in Azure
- Provide a dedicated security testing environment
- Support CI/CD deployment workflows
- Demonstrate enterprise networking and cloud administration

---

# High-Level Architecture

```
                           Internet
                               │
                        Cloudflare DNS
                               │
                               ▼
                  AWS Production Environment
                 10.0.0.0/16
          ┌───────────────────────────┐
          │ Application Load Balancer │
          │ React Frontend            │
          │ FastAPI Backend           │
          │ PostgreSQL Database       │
          │ Amazon S3                │
          │ CloudWatch               │
          │ AWS IAM                  │
          │ WireGuard VPN Hub        │
          └───────────────────────────┘
                   ▲              ▲
                   │              │
          WireGuard VPN   WireGuard VPN
                   │              │
                   ▼              ▼

      Azure Corporate IT     On-Premises Security Lab
          10.1.0.0/16             10.2.0.0/16
```

---

# Environment Responsibilities

## AWS Production Environment

AWS hosts all customer-facing services.

### Responsibilities

- Production web application
- API hosting
- Database hosting
- Cloud storage
- Monitoring
- VPN hub
- Cloud identity and access management

### Services

- Amazon EC2
- WireGuard VPN Hub
- Application Load Balancer
- React Frontend
- FastAPI Backend
- PostgreSQL
- Amazon S3
- CloudWatch
- AWS IAM

---

## Azure Corporate Environment

Azure represents the organization's corporate IT infrastructure.

### Responsibilities

- Enterprise identity
- Active Directory
- DNS
- Group Policy
- File services
- Developer workstation
- Internal infrastructure

### Services

- Ubuntu WireGuard Gateway
- Windows Server 2025
- Active Directory Domain Services
- DNS
- Windows File Server
- Internal Application Server
- Windows 11 Developer Workstation

---

## On-Premises Security Lab

The on-premises environment simulates an enterprise security operations lab.

### Responsibilities

- Security testing
- Vulnerability assessment
- Penetration testing
- Network analysis
- Application validation

### Services

- Ubuntu WireGuard Gateway
- Kali Linux
- Security Test Workstation

### Security Tools

- Burp Suite Community
- OWASP ZAP
- Nmap
- Wireshark
- Custom Python Security Scripts

---

# Network Segmentation

## AWS

| Subnet | Purpose |
|---------|----------|
| Public Subnet | Internet-facing services |
| Private Application Subnet | React / FastAPI |
| Private Database Subnet | PostgreSQL |

Network

```
10.0.0.0/16
```

---

## Azure

| Subnet | Purpose |
|---------|----------|
| Public Subnet | WireGuard Gateway |
| Identity Services Subnet | Active Directory & DNS |
| Developer Subnet | Windows 11 Developer VM |
| Infrastructure Services Subnet | File Server & Internal Applications |

Network

```
10.1.0.0/16
```

---

## On-Premises

| Subnet | Purpose |
|---------|----------|
| Public Subnet | WireGuard Gateway |
| Security Operations Subnet | Kali Linux |
| Security Testing Subnet | Test Workstation |

Network

```
10.2.0.0/16
```

---

# VPN Architecture

The environments communicate using a WireGuard hub-and-spoke VPN topology.

```
               AWS WireGuard Hub
              172.16.100.1
                 /       \
                /         \
               /           \
              ▼             ▼

Azure Gateway         On-Prem Gateway
172.16.100.2          172.16.100.3
```

AWS serves as the central routing point for all VPN-connected environments.

---

# Identity Architecture

Corporate identities are managed within Azure.

Users authenticate against:

- Active Directory Domain Services
- DNS
- Group Policy

Example accounts

```
Eric.Admin

John.Developer
```

The Windows 11 Developer Workstation is joined to the Active Directory domain.

---

# Application Workflow

```
Windows 11 Developer Workstation
        │
        ▼
Visual Studio Code
        │
        ▼
GitHub Repository
        │
        ▼
GitHub Actions
        │
        ▼
AWS Production
        │
        ▼
Customers
```

---

# Security Workflow

The on-premises security lab validates the production application after deployment.

```
AWS Production
        │
        ▼
Kali Linux
        │
        ▼
Burp Suite

OWASP ZAP

Nmap

Wireshark

Security Test Workstation
```

---

# Network Security

Security controls include:

- WireGuard Site-to-Site VPN
- Security Groups
- Network Security Groups
- Linux Firewall Rules
- Principle of Least Privilege
- AWS IAM
- Active Directory Security Groups

---

# Current Implementation Status

## Completed

- AWS Virtual Network
- Azure Virtual Network
- On-Premises Network
- WireGuard Hub-and-Spoke VPN
- AWS ↔ Azure Connectivity
- AWS ↔ On-Premises Connectivity
- Routing Configuration
- Architecture Documentation
- Network Diagrams

---

## In Progress

- Windows Server 2025
- Active Directory
- DNS
- Windows 11 Developer Workstation
- File Server

---

## Planned

- React Frontend
- FastAPI Backend
- PostgreSQL
- GitHub Actions
- HTTPS
- Monitoring
- Automated Deployment
- Security Testing Pipeline

---

# Future Enhancements

- Microsoft Entra ID
- AWS IAM Identity Center
- Docker
- Kubernetes
- Prometheus
- Grafana
- SIEM
- Centralized Logging
- Web Application Firewall
- Infrastructure as Code (Terraform)

---

# Summary

The Enterprise Hybrid Cloud Platform combines AWS, Azure, and an on-premises security lab into a unified enterprise environment.

The architecture demonstrates modern hybrid cloud design by separating production services, enterprise identity management, and security operations into dedicated environments while maintaining secure communication through a WireGuard hub-and-spoke VPN.