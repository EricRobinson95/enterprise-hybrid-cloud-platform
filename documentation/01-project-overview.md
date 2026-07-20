# 01 - Project Overview

# Enterprise Hybrid Cloud Platform

## Overview

The Enterprise Hybrid Cloud Platform is a real-world hybrid cloud infrastructure project designed to demonstrate enterprise networking, cloud engineering, Windows administration, Linux administration, DevOps, and cybersecurity.

The project combines Amazon Web Services (AWS), Microsoft Azure, and an on-premises security lab into a single enterprise environment connected through a secure WireGuard site-to-site VPN.

Rather than focusing on a single technology, the project demonstrates how multiple enterprise platforms work together to support application development, production hosting, identity management, and security operations.

---

# Project Objectives

The primary objectives of this project are to:

- Design a scalable hybrid cloud architecture
- Build secure site-to-site VPN connectivity
- Deploy enterprise Active Directory infrastructure
- Create a managed developer environment
- Build and deploy a production-ready web application
- Implement CI/CD using GitHub Actions
- Perform security testing from an isolated security lab
- Document every phase of the implementation

---

# Project Architecture

The environment is divided into three primary locations.

## AWS Production Environment

AWS hosts all customer-facing services and acts as the central networking hub.

Responsibilities include:

- Production Web Application
- React Frontend
- FastAPI Backend
- PostgreSQL Database
- Application Load Balancer
- Amazon S3
- CloudWatch
- AWS IAM
- WireGuard VPN Hub

Network

```
10.0.0.0/16
```

---

## Azure Corporate Environment

Azure represents the organization's corporate IT infrastructure.

Responsibilities include:

- Active Directory Domain Services
- DNS
- Group Policy
- Windows Server 2025
- Windows File Server
- Internal Application Server
- Windows 11 Developer Workstation
- Enterprise User Management

Network

```
10.1.0.0/16
```

---

## On-Premises Security Lab

The on-premises environment represents the organization's security operations and testing environment.

Responsibilities include:

- WireGuard VPN Gateway
- Kali Linux
- Security Test Workstation
- Penetration Testing
- Vulnerability Assessment
- Network Analysis
- Application Security Validation

Network

```
10.2.0.0/16
```

---

# Hybrid Cloud Design

```
                           Internet
                               │
                         Cloudflare DNS
                               │
                               ▼
                  AWS Production Environment
                React • FastAPI • PostgreSQL
                     WireGuard VPN Hub
                         10.0.0.0/16
                        /             \
                       /               \
                      ▼                 ▼

       Azure Corporate IT       On-Premises Security Lab
            10.1.0.0/16              10.2.0.0/16
```

---

# Enterprise Workflow

The project simulates the workflow of a modern software development organization.

## Corporate IT

Enterprise user accounts are created and managed within Active Directory.

Example users:

- Eric.Admin
- John.Developer

---

## Software Development

Developers authenticate to a domain-joined Windows 11 workstation hosted in Azure.

Development tools include:

- Visual Studio Code
- Git
- Python
- Node.js
- Docker (Future)

---

## Source Control

Application source code is maintained in GitHub.

Development follows standard Git workflows including:

- Feature development
- Version control
- Pull requests
- Source history

---

## Continuous Integration

GitHub Actions automatically builds and deploys the application to AWS.

Deployment workflow:

```
Developer

↓

GitHub Repository

↓

GitHub Actions

↓

AWS Production
```

---

## Production Hosting

AWS hosts the production application.

Customers access the application through:

```
Internet

↓

Cloudflare

↓

AWS Production
```

---

## Security Operations

After deployment, the application is validated using the on-premises security lab.

Security testing includes:

- Network Discovery
- Port Enumeration
- Burp Suite Testing
- OWASP ZAP
- Wireshark Analysis
- API Testing
- HTTPS Validation
- Vulnerability Assessment

---

# Core Technologies

## Cloud Platforms

- Amazon Web Services
- Microsoft Azure
- Cloudflare

---

## Networking

- WireGuard
- Site-to-Site VPN
- Linux Routing
- TCP/IP
- Static Routing

---

## Operating Systems

- Ubuntu Server
- Windows Server 2025
- Windows 11
- Kali Linux

---

## Identity

- Active Directory Domain Services
- DNS
- Group Policy
- AWS IAM

---

## Development

- Visual Studio Code
- Git
- GitHub
- GitHub Actions
- Python
- React
- FastAPI
- PostgreSQL

---

## Security

- Burp Suite Community
- OWASP ZAP
- Wireshark
- Nmap
- Custom Python Security Tools

---

# Current Progress

## Completed

- Hybrid cloud architecture design
- AWS virtual network
- Azure virtual network
- On-premises network
- WireGuard hub-and-spoke VPN
- AWS ↔ Azure VPN connectivity
- AWS ↔ On-Premises VPN connectivity
- Static routing
- Linux IP forwarding
- Network diagrams
- Project documentation

---

## Current Phase

**Phase 2 – Enterprise Identity Services**

Current work focuses on building the Azure corporate environment, including:

- Windows Server 2025
- Active Directory
- DNS
- Group Policy
- Windows 11 Developer Workstation

---

## Future Phases

- Web Application Development
- AWS Production Deployment
- CI/CD Pipeline
- Security Operations
- Monitoring
- Infrastructure Automation

---

# Learning Objectives

This project provides hands-on experience with:

- Enterprise Networking
- Hybrid Cloud Architecture
- Windows Server Administration
- Linux Administration
- Active Directory
- Cloud Infrastructure
- DevOps
- Git
- CI/CD
- Web Application Deployment
- Cybersecurity
- Infrastructure Documentation

---

# Project Documentation

The repository contains detailed documentation covering:

- Project Planning
- Network Architecture
- VPN Configuration
- Routing
- Azure Infrastructure
- AWS Infrastructure
- Security
- Testing & Validation
- Troubleshooting
- Future Roadmap

---

# Summary

The Enterprise Hybrid Cloud Platform is a comprehensive infrastructure project that demonstrates how AWS, Azure, and an on-premises security lab can be integrated into a modern enterprise environment.

The project follows industry practices by separating production services, enterprise identity, software development, and security operations into dedicated environments while documenting every stage of implementation. It serves as a practical portfolio demonstrating hybrid cloud architecture, enterprise networking, systems administration, DevOps, and cybersecurity.