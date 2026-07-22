# Project Overview

# Enterprise Hybrid Cloud Platform

## Overview

The Enterprise Hybrid Cloud Platform is a production-style infrastructure project designed to demonstrate enterprise networking, hybrid cloud architecture, Windows Server administration, Linux administration, DevOps, and cybersecurity.

The project integrates Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware environment into a unified enterprise infrastructure connected through a secure WireGuard hub-and-spoke VPN.

Rather than focusing on a single technology, the project demonstrates how multiple enterprise platforms work together to provide secure networking, centralized identity management, cloud infrastructure, application hosting, and security operations.

---

# Project Objectives

The primary objectives of this project are to:

- Design a scalable hybrid cloud architecture
- Build a secure WireGuard site-to-site VPN
- Implement centralized enterprise identity management
- Deploy Windows Server 2025 Active Directory
- Build a managed Windows enterprise environment
- Deploy a production-ready web application
- Implement CI/CD with GitHub Actions
- Perform security testing from an isolated security lab
- Document every phase of the implementation

---

# Project Architecture

The environment is divided into three primary locations.

---

## AWS Production Environment

AWS hosts the production application infrastructure and serves as the central networking hub.

Current Responsibilities

- WireGuard VPN Hub
- Linux Routing
- VPN Packet Forwarding
- Hybrid Cloud Connectivity

Planned Services

- React Frontend
- FastAPI Backend
- PostgreSQL Database
- Amazon ECS
- Application Load Balancer
- Amazon S3
- CloudWatch

Network

```
10.0.0.0/16
```

---

## Azure Enterprise Environment

Azure represents the organization's corporate infrastructure.

Current Responsibilities

- Windows Server 2025
- Active Directory Domain Services
- DNS
- Enterprise User Management
- Security Groups
- WireGuard VPN Gateway

Planned Services

- Windows File Server
- Internal Application Server
- Windows 11 Enterprise Workstation
- Group Policy

Network

```
10.1.0.0/16
```

---

## On-Premises Security Lab

The on-premises environment simulates an enterprise office and security operations lab.

Current Responsibilities

- Ubuntu WireGuard Gateway
- Kali Linux
- VMware Infrastructure
- Secure Remote Administration

Planned Services

- Windows 11 Enterprise Client
- Burp Suite Testing
- OWASP ZAP
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
                 WireGuard VPN Hub (10.0.0.0/16)
                               │
                ┌──────────────┴──────────────┐
                │                             │
                ▼                             ▼

      Azure Enterprise              On-Premises Security Lab
          10.1.0.0/16                    10.2.0.0/16

      Active Directory                 Kali Linux
      Windows Server 2025              VMware
      DNS                              WireGuard Gateway
```

---

# Enterprise Workflow

The project simulates the workflow of a modern enterprise organization.

## Enterprise Identity

Users are centrally managed through Active Directory hosted in Azure.

Current Implementation

- Active Directory Domain
- Organizational Units
- Administrative User
- Security Group

Future

- Domain-joined Windows 11 clients
- Group Policy
- Enterprise authentication

---

## Software Development

Developers will authenticate to domain-joined Windows 11 workstations.

Development tools include:

- Visual Studio Code
- Git
- Python
- Node.js
- Docker (Future)

---

## Source Control

Application source code is maintained using GitHub.

Development workflow includes:

- Feature branches
- Version control
- Pull requests
- Code review
- Documentation

---

## Continuous Integration

GitHub Actions will automate application deployment.

Deployment workflow

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

AWS will host the production application.

Application architecture

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

## Security Operations

The on-premises security lab will validate the deployed application.

Planned testing includes:

- Network Discovery
- Port Enumeration
- Burp Suite
- OWASP ZAP
- Wireshark
- API Testing
- HTTPS Validation
- Vulnerability Assessment

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
- Custom Python Security Tools

---

# Current Progress

## Completed

### Networking

- AWS Virtual Private Cloud
- Azure Virtual Network
- On-Premises VMware Network
- Hub-and-Spoke WireGuard VPN
- AWS ↔ Azure Connectivity
- AWS ↔ On-Premises Connectivity
- Azure ↔ On-Premises Connectivity
- Linux Routing
- Packet Forwarding
- Network Validation

### Identity

- Windows Server 2025 Deployment
- Active Directory Domain Services
- DNS
- Organizational Units
- Administrative User
- Administrative Security Group

### Documentation

- Architecture Diagrams
- Infrastructure Documentation
- VPN Documentation
- Security Documentation
- Validation Documentation

---

## Current Phase

**Phase 3 – Enterprise Workstations**

Current work focuses on:

- Azure Windows 11 Enterprise Client
- On-Premises Windows 11 Enterprise Client
- Domain Join
- Group Policy
- Cross-site Authentication

---

## Future Phases

- File Server
- Internal Application Server
- Web Application Development
- CI/CD Pipeline
- Security Testing
- Monitoring
- Infrastructure Automation

---

# Learning Objectives

This project provides practical experience with:

- Enterprise Networking
- Hybrid Cloud Architecture
- WireGuard VPN
- Windows Server Administration
- Active Directory
- Linux Administration
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

- Project Planning
- Network Architecture
- IP Addressing
- AWS Infrastructure
- Azure Infrastructure
- VPN Architecture
- WireGuard Implementation
- Active Directory
- Security
- Testing and Validation
- Future Roadmap

---

# Summary

The Enterprise Hybrid Cloud Platform demonstrates how AWS, Microsoft Azure, and an on-premises enterprise environment can be integrated into a secure hybrid cloud infrastructure.

The networking foundation has been completed with a fully operational WireGuard hub-and-spoke VPN, centralized Active Directory services, enterprise DNS, and validated cross-site connectivity. The remaining project phases focus on Windows client deployment, enterprise identity integration, application hosting, DevOps automation, and security testing.

The completed project will showcase practical experience in hybrid cloud architecture, enterprise networking, Windows administration, Linux administration, DevOps, and cybersecurity while serving as a comprehensive portfolio demonstrating real-world infrastructure engineering.