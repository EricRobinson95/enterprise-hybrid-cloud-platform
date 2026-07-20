# Enterprise Hybrid Cloud Platform

A real-world hybrid cloud infrastructure project demonstrating enterprise networking, cloud architecture, Windows administration, Linux administration, DevOps, and cybersecurity.

The environment connects Amazon Web Services (AWS), Microsoft Azure, and an on-premises security lab using a WireGuard hub-and-spoke VPN topology. The long-term objective is to build and deploy a production-ready web application while managing enterprise identity, development, and security testing across multiple environments.

---

# Project Goals

- Design a real-world hybrid cloud architecture
- Build secure site-to-site VPN connectivity
- Deploy enterprise Active Directory infrastructure
- Develop and deploy a production web application
- Implement CI/CD with GitHub Actions
- Perform security testing from an isolated on-premises lab
- Document the complete deployment and configuration process

---

# Architecture Overview

## AWS Production Environment

AWS hosts the production application and serves as the central networking hub.

Services include:

- Amazon EC2
- WireGuard VPN Hub
- React Frontend
- FastAPI Backend
- PostgreSQL Database
- Application Load Balancer
- Amazon S3
- AWS IAM
- CloudWatch

Production Network

```
10.0.0.0/16
```

---

## Azure Corporate Environment

Azure represents the company's corporate IT infrastructure.

Current / Planned Services

- Ubuntu WireGuard Gateway
- Windows Server 2025
- Active Directory Domain Services
- DNS
- Group Policy
- Windows File Server
- Internal Application Server
- Windows 11 Developer Workstation

Corporate Network

```
10.1.0.0/16
```

---

## On-Premises Security Lab

The on-premises environment simulates the organization's security operations lab.

Services

- Ubuntu WireGuard Gateway
- Kali Linux
- Security Testing Workstation

Security tools include

- Burp Suite
- Wireshark
- Nmap
- OWASP ZAP
- Custom Python Security Scripts

On-Premises Network

```
10.2.0.0/16
```

---

# Hybrid Network Topology

```
                    Internet
                        │
                  Cloudflare DNS
                        │
                        ▼
             AWS Production Environment
          (WireGuard Hub + Web Application)
                 10.0.0.0/16
                 /          \
                /            \
               /              \
              ▼                ▼

Azure Corporate        On-Premises Security Lab
10.1.0.0/16            10.2.0.0/16

Windows Server         Ubuntu WireGuard
Windows 11             Kali Linux
Developer VM           Security Testing
```

---

# Current Project Status

## Completed

- AWS Virtual Private Cloud
- Azure Virtual Network
- On-Premises Network
- WireGuard Hub-and-Spoke VPN
- AWS ↔ Azure VPN Connectivity
- AWS ↔ On-Premises VPN Connectivity
- Linux Routing Configuration
- IP Forwarding Configuration
- Architecture Documentation
- Network Diagrams

---

## In Progress

- Azure Active Directory Infrastructure
- Windows Server 2025
- DNS
- Organizational Units
- Users & Groups
- Windows 11 Developer Workstation

---

## Planned

- React Frontend
- FastAPI Backend
- PostgreSQL
- GitHub Actions CI/CD
- CloudWatch Monitoring
- Security Testing Pipeline
- HTTPS
- Production Deployment

---

# Repository Structure

```
enterprise-hybrid-cloud-platform/

├── configs/
│   ├── linux/
│   ├── wireguard/
│   └── cloud-init/
│
├── diagrams/
│
├── documentation/
│
├── images/
│
├── scripts/
│
└── README.md
```

---

# Technologies

## Cloud

- Amazon Web Services
- Microsoft Azure
- Cloudflare

## Networking

- WireGuard
- IPv4 Routing
- Site-to-Site VPN
- Linux Networking
- TCP/IP

## Operating Systems

- Ubuntu Server
- Windows Server 2025
- Windows 11
- Kali Linux

## Identity

- Active Directory Domain Services
- DNS
- Group Policy
- AWS IAM

## Development

- Git
- GitHub
- GitHub Actions
- Visual Studio Code
- Python
- React
- FastAPI

## Security

- Burp Suite
- Wireshark
- Nmap
- OWASP ZAP

---

# Documentation

Detailed documentation is available in the `documentation/` directory, including:

- Project Overview
- Business Requirements
- Network Architecture
- IP Addressing
- WireGuard Configuration
- Routing
- Security
- Verification
- Troubleshooting

---

# Future Enhancements

- Microsoft Entra ID integration
- AWS IAM Identity Center
- Automated Infrastructure Deployment
- Docker Containers
- Kubernetes
- Prometheus
- Grafana
- SIEM Integration
- Centralized Logging
- Web Application Firewall (WAF)

---

# License

This project is licensed under the MIT License.