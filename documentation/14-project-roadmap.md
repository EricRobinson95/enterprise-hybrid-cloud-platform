# 14 - Project Roadmap

# Overview

This roadmap outlines the development phases of the Enterprise Hybrid Cloud Platform.

The project progresses from building the underlying hybrid cloud infrastructure to deploying a production-ready web application, implementing enterprise identity services, establishing CI/CD, and performing security validation.

Each phase builds upon the previous one to simulate a real-world enterprise environment.

---

# Current Project Status

## Current Phase

**Phase 2 – Enterprise Identity Services**

The hybrid cloud networking foundation has been completed, and development is now focused on building the corporate IT environment in Microsoft Azure.

---

# Project Timeline

| Phase | Status |
|--------|--------|
| Phase 1 – Hybrid Cloud Networking | ✅ Complete |
| Phase 2 – Enterprise Identity Services | 🔄 In Progress |
| Phase 3 – Developer Environment | ⏳ Planned |
| Phase 4 – Web Application Development | ⏳ Planned |
| Phase 5 – AWS Production Deployment | ⏳ Planned |
| Phase 6 – CI/CD Pipeline | ⏳ Planned |
| Phase 7 – Security Operations | ⏳ Planned |
| Phase 8 – Monitoring & Operations | ⏳ Planned |
| Phase 9 – Future Enhancements | ⏳ Planned |

---

# Phase 1 – Hybrid Cloud Networking

## Objectives

Build the networking foundation connecting AWS, Azure, and the on-premises security lab.

## Completed

- AWS Virtual Private Cloud
- Azure Virtual Network
- On-Premises Network
- WireGuard Hub
- Azure WireGuard Gateway
- On-Premises WireGuard Gateway
- Hub-and-Spoke VPN Topology
- Static Routing
- Linux IP Forwarding
- Route Verification
- Packet Capture Validation
- Architecture Documentation
- Network Diagrams

## Deliverables

- Secure site-to-site VPN
- Hybrid cloud architecture
- Routing documentation
- Network validation

**Status:** ✅ Complete

---

# Phase 2 – Enterprise Identity Services

## Objectives

Build the organization's corporate IT environment within Azure.

## Tasks

### Azure Infrastructure

- Deploy Windows Server 2025
- Configure static private IP
- Configure DNS

### Active Directory

- Install Active Directory Domain Services
- Create a new forest
- Create Organizational Units
- Create Security Groups
- Configure Group Policy

### User Management

Create enterprise accounts including:

- Eric.Admin
- John.Developer

### Infrastructure Services

- Deploy Windows File Server
- Deploy Internal Application Server

## Deliverables

- Enterprise Active Directory
- DNS
- Group Policy
- User Accounts
- Security Groups

**Status:** 🔄 In Progress

---

# Phase 3 – Developer Environment

## Objectives

Build a managed Windows development workstation for enterprise software development.

## Tasks

- Deploy Windows 11 VM
- Join Windows 11 to the domain
- Configure Remote Desktop
- Install Visual Studio Code
- Install Git
- Install Python
- Install Node.js
- Install Docker Desktop
- Install AWS CLI
- Install Azure CLI

## Deliverables

- Domain-joined developer workstation
- Standardized development environment
- Remote development capability

**Status:** ⏳ Planned

---

# Phase 4 – Web Application Development

## Objectives

Develop a production-ready web application.

## Frontend

- React
- HTML
- CSS
- JavaScript

## Backend

- FastAPI
- Python
- REST API

## Database

- PostgreSQL

## Authentication

- Application user authentication
- Role-based authorization

## Deliverables

- Production web application
- REST API
- Database integration

**Status:** ⏳ Planned

---

# Phase 5 – AWS Production Deployment

## Objectives

Deploy the application into the AWS production environment.

## Tasks

- Deploy EC2 Application Server
- Configure Application Load Balancer
- Deploy PostgreSQL
- Configure Security Groups
- Configure IAM Roles
- Configure Amazon S3
- Configure HTTPS

## Deliverables

- Production deployment
- Public web application
- Secure cloud infrastructure

**Status:** ⏳ Planned

---

# Phase 6 – CI/CD Pipeline

## Objectives

Automate application deployment.

## Tasks

- GitHub Repository
- GitHub Actions
- Automated Build
- Automated Testing
- Automated Deployment

## Deployment Workflow

```
Developer
      │
      ▼
GitHub Repository
      │
      ▼
GitHub Actions
      │
      ▼
AWS Production
```

## Deliverables

- Automated deployments
- Continuous Integration
- Continuous Delivery

**Status:** ⏳ Planned

---

# Phase 7 – Security Operations

## Objectives

Validate the production environment using the on-premises security lab.

## Security Testing

- Network Discovery
- Port Enumeration
- Web Application Testing
- Authentication Testing
- API Testing
- HTTPS Validation
- Security Header Validation
- Vulnerability Assessment

## Security Tools

- Kali Linux
- Burp Suite Community
- OWASP ZAP
- Nmap
- Wireshark
- Custom Python Security Scripts

## Deliverables

- Security Assessment
- Vulnerability Report
- Remediation Documentation

**Status:** ⏳ Planned

---

# Phase 8 – Monitoring & Operations

## Objectives

Implement operational monitoring and observability.

## AWS

- CloudWatch
- CloudWatch Logs
- Metrics
- Alerts

## Azure

- Azure Monitor
- Backup
- Update Management

## Deliverables

- Monitoring Dashboard
- Operational Alerts
- Performance Metrics

**Status:** ⏳ Planned

---

# Phase 9 – Future Enhancements

Future improvements planned after the core platform is complete.

## Cloud

- Microsoft Entra ID
- AWS IAM Identity Center
- Azure Bastion
- Azure Backup
- Azure Key Vault

## DevOps

- Docker
- Kubernetes
- Terraform
- Infrastructure as Code

## Monitoring

- Prometheus
- Grafana
- Centralized Logging

## Security

- SIEM
- WAF
- Automated Vulnerability Scanning
- Secret Management

**Status:** ⏳ Planned

---

# Project Success Criteria

The project will be considered complete when it demonstrates:

## Infrastructure

- Hybrid cloud architecture
- Enterprise networking
- Secure site-to-site VPN

## Identity

- Active Directory
- Group Policy
- Enterprise user management

## Development

- Domain-joined developer workstation
- Modern development workflow

## Application

- React frontend
- FastAPI backend
- PostgreSQL database

## Cloud

- AWS production deployment
- Azure corporate infrastructure

## DevOps

- GitHub Actions
- Automated deployment

## Security

- Security testing
- Vulnerability assessment
- Production validation

---

# Summary

The Enterprise Hybrid Cloud Platform is being developed in structured phases that mirror the lifecycle of a real enterprise deployment.

Beginning with secure hybrid networking, the project progresses through enterprise identity management, application development, cloud deployment, DevOps automation, and cybersecurity validation. This phased approach ensures that each component is fully implemented, documented, and tested before moving to the next stage, resulting in a comprehensive portfolio demonstrating networking, cloud engineering, Windows administration, Linux administration, software development, DevOps, and security.