# Enterprise Hybrid Cloud Platform

## Overview

The Enterprise Hybrid Cloud Platform is a production-style infrastructure project that demonstrates enterprise networking, hybrid cloud architecture, Windows Server administration, Linux administration, cloud engineering, DevOps, and cybersecurity.

The environment integrates Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware infrastructure into a secure hybrid cloud connected through a WireGuard hub-and-spoke VPN.

The project is designed to simulate a real enterprise environment where cloud services, centralized identity management, routing, security, and application hosting work together across multiple platforms.

---

# Project Objectives

The primary objectives of this project are to:

- Design a scalable hybrid cloud architecture
- Build a secure WireGuard hub-and-spoke VPN
- Implement enterprise routing between multiple networks
- Deploy Windows Server 2025 Active Directory
- Implement centralized enterprise identity management
- Deploy Windows 11 enterprise workstations
- Build a production-ready web application
- Implement CI/CD with GitHub Actions
- Perform application security testing
- Document the complete infrastructure deployment

---

# Enterprise Architecture

The environment consists of three enterprise locations connected through secure site-to-site VPN tunnels.

```
                        Internet
                            │
                     Cloudflare DNS
                            │
                            ▼
                  AWS Production Environment
                  WireGuard VPN Hub
                      10.0.0.0/16
                            │
           ┌────────────────┴────────────────┐
           │                                 │
           ▼                                 ▼

 Azure Enterprise Environment      On-Premises Enterprise Lab
        10.1.0.0/16                     10.2.0.0/16

 Active Directory                 VMware Infrastructure
 Windows Server 2025              Windows 11 Enterprise
 DNS                              Ubuntu WireGuard Gateway
 Identity Services                Kali Linux Security Lab
```

---

# Infrastructure Overview

## AWS Production Environment

AWS serves as the production cloud environment and central networking hub.

### Current Infrastructure

- Amazon VPC
- Public Subnet
- Private Routing
- Internet Gateway
- Route Tables
- Ubuntu WireGuard Gateway
- Linux IP Forwarding
- Static Routing

### Planned Services

- React Frontend
- FastAPI Backend
- PostgreSQL Database
- Amazon ECS
- Application Load Balancer
- Amazon S3
- CloudWatch
- GitHub Actions Deployment

Network

```
10.0.0.0/16
```

---

## Azure Enterprise Environment

Azure represents the corporate datacenter.

### Current Infrastructure

- Azure Virtual Network
- Windows Server 2025
- Active Directory Domain Services
- DNS
- Organizational Units
- Enterprise Users
- Security Groups
- Azure WireGuard Gateway
- User Defined Routes (UDRs)

### Planned Services

- Windows File Server
- Internal Application Server
- Group Policy
- Roaming Enterprise Workstations

Network

```
10.1.0.0/16
```

---

## On-Premises Enterprise Lab

The on-premises environment simulates an enterprise office and security operations center.

### Current Infrastructure

- VMware Workstation
- Ubuntu WireGuard Gateway
- Windows 11 Enterprise Workstation
- Kali Linux Security VM
- Enterprise Routing
- Static Routes

### Planned Services

- Burp Suite Testing
- OWASP ZAP
- Wireshark
- Network Monitoring
- API Security Testing

Network

```
10.2.0.0/16
```

---

# Enterprise Networking

The networking infrastructure provides secure communication between all enterprise environments.

Current implementation includes:

- WireGuard Hub-and-Spoke VPN
- Site-to-Site VPN Connectivity
- Linux IP Forwarding
- Static Routing
- AWS Route Tables
- Azure User Defined Routes
- Windows Persistent Routes
- Cross-Site DNS Resolution
- End-to-End Network Validation

Validated communication includes:

- AWS ↔ Azure
- AWS ↔ On-Premises
- Azure ↔ On-Premises

---

# Enterprise Identity

Identity services are hosted within Azure using Active Directory Domain Services.

Current implementation includes:

- Active Directory Domain
- Organizational Units
- Administrative Accounts
- Enterprise User Accounts
- Security Groups
- DNS
- Domain-Joined Windows 11 Workstations

Future implementation includes:

- Group Policy
- File Server Authentication
- Centralized Policy Management

---

# Software Development

Application development will occur from enterprise Windows workstations using modern development tools.

Development stack:

- Visual Studio Code
- Git
- GitHub
- Python
- Node.js
- React
- FastAPI
- PostgreSQL

---

# DevOps Pipeline

Application deployment will be automated using GitHub Actions.

Deployment workflow

```
Developer

↓

GitHub

↓

GitHub Actions

↓

AWS Production Environment
```

---

# Production Application

The production environment will host a modern web application.

Architecture

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

PostgreSQL Database
```

---

# Security Operations

The on-premises security lab will validate the deployed infrastructure and applications.

Planned testing includes:

- Nmap
- Burp Suite Community
- OWASP ZAP
- Wireshark
- API Testing
- HTTPS Validation
- Vulnerability Assessment
- Custom Python Security Tools

---

# Core Technologies

## Cloud

- Amazon Web Services
- Microsoft Azure
- Cloudflare

---

## Networking

- WireGuard
- Hub-and-Spoke VPN
- Linux Routing
- IP Forwarding
- Static Routing
- TCP/IP

---

## Operating Systems

- Ubuntu Server
- Windows Server 2025
- Windows 11 Enterprise
- Kali Linux

---

## Identity

- Active Directory Domain Services
- DNS
- Organizational Units
- Security Groups
- Windows Authentication
- AWS IAM

---

## Development

- Git
- GitHub
- GitHub Actions
- Visual Studio Code
- Python
- React
- FastAPI
- PostgreSQL

---

## Security

- Kali Linux
- Burp Suite Community
- OWASP ZAP
- Wireshark
- Nmap
- Python Security Tools

---

# Current Progress

## Completed

### Networking

- AWS Production VPC
- Azure Enterprise VNet
- On-Premises VMware Network
- WireGuard Hub-and-Spoke VPN
- AWS Route Tables
- Azure User Defined Routes
- Windows Static Routes
- Linux Packet Forwarding
- Cross-Site Routing
- Cross-Site DNS Resolution
- End-to-End Network Validation

### Identity

- Windows Server 2025 Deployment
- Active Directory Domain Services
- DNS
- Organizational Units
- Enterprise User Accounts
- Administrative Accounts
- Security Groups
- Domain-Joined Windows 11 Workstation

### Infrastructure

- AWS WireGuard Gateway
- Azure WireGuard Gateway
- Ubuntu On-Premises Gateway
- VMware Enterprise Lab

### Documentation

- Architecture Diagrams
- AWS Infrastructure
- Azure Infrastructure
- WireGuard Configuration
- Routing Documentation
- Active Directory Documentation
- Testing and Validation

---

# Current Phase

**Phase 4 – Enterprise Infrastructure**

Current work focuses on:

- Windows File Server
- Group Policy
- Internal Services
- Production Infrastructure Preparation

---

# Future Phases

- Internal File Server
- Enterprise Group Policy
- Production Web Application
- CI/CD Pipeline
- Security Testing
- Infrastructure Monitoring
- Infrastructure Automation

---

# Learning Objectives

This project provides practical experience with:

- Enterprise Networking
- Hybrid Cloud Architecture
- WireGuard VPN
- Linux Administration
- Windows Server Administration
- Active Directory
- Enterprise Routing
- Cloud Infrastructure
- DevOps
- Git
- GitHub Actions
- CI/CD
- Application Deployment
- Cybersecurity
- Infrastructure Documentation

---

# Repository Documentation

The repository includes detailed documentation covering:

- Project Overview
- Architecture
- AWS Infrastructure
- Azure Infrastructure
- On-Premises Infrastructure
- Routing
- WireGuard
- Active Directory
- Windows Workstations
- Security
- Testing and Validation
- Future Roadmap

---

# Summary

The Enterprise Hybrid Cloud Platform demonstrates how AWS, Microsoft Azure, and an on-premises enterprise environment can be integrated into a secure production-style hybrid cloud infrastructure.

The networking foundation has been completed with a fully operational WireGuard hub-and-spoke VPN, enterprise routing, centralized Active Directory, DNS, cross-site authentication, domain-joined Windows workstations, and validated connectivity between all environments.

Future phases will expand the environment with enterprise file services, Group Policy, application hosting, CI/CD automation, monitoring, and application security testing.

The completed project will showcase practical experience across cloud engineering, enterprise networking, Windows administration, Linux administration, DevOps, and cybersecurity while serving as a comprehensive portfolio demonstrating real-world infrastructure engineering.