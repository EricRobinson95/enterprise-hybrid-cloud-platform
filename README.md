# Enterprise Hybrid Cloud Platform

> **A production-style hybrid cloud infrastructure connecting AWS, Microsoft Azure, and an on-premises enterprise network through a secure WireGuard hub-and-spoke VPN.**

![Status](https://img.shields.io/badge/Status-Complete-success)
![AWS](https://img.shields.io/badge/AWS-Deployed-orange)
![Azure](https://img.shields.io/badge/Azure-Deployed-blue)
![WireGuard](https://img.shields.io/badge/WireGuard-Operational-green)
![Active%20Directory](https://img.shields.io/badge/Active%20Directory-Operational-success)

---

# Overview

The **Enterprise Hybrid Cloud Platform** is a production-style infrastructure engineering project that demonstrates how modern organizations securely integrate **Amazon Web Services (AWS)**, **Microsoft Azure**, and **on-premises infrastructure** into a unified enterprise network.

Rather than focusing on a single technology, this project combines enterprise networking, hybrid cloud architecture, Active Directory, Windows Server administration, Linux administration, VPN technologies, routing, and cybersecurity into one complete infrastructure.

The completed environment provides:

- Secure hybrid cloud networking
- Centralized enterprise identity management
- Cross-site authentication
- Domain-joined Windows workstations
- Enterprise DNS
- Secure remote administration
- Comprehensive infrastructure documentation

This repository represents the completed infrastructure foundation for future enterprise automation, application hosting, and DevOps projects.

---

# Enterprise Architecture

![Enterprise Architecture](/images/diagrams/logical-diagram.png)


# Infrastructure Highlights

## Enterprise Networking

Designed and implemented a secure hybrid cloud network connecting three independent environments.

### Features

- WireGuard Hub-and-Spoke VPN
- Site-to-Site VPN
- Enterprise Routing
- Static Routes
- Linux Packet Forwarding
- Network Segmentation
- Cross-Site DNS Resolution
- Cross-Site Authentication

Validated connectivity between:

- AWS ↔ Azure
- AWS ↔ On-Premises
- Azure ↔ On-Premises

---

## Active Directory

Built a centralized enterprise identity platform hosted in Microsoft Azure.

### Implemented

- Windows Server 2025
- Active Directory Domain Services
- Enterprise DNS
- Organizational Units
- Security Groups
- Administrative Accounts
- Developer Accounts
- Domain Authentication

---

## Enterprise Workstations

Successfully deployed enterprise Windows clients.

### Completed

- Azure Windows 11 Enterprise Workstation
- On-Premises Windows 11 Enterprise Workstation
- Domain Join
- Cross-Site Authentication
- Remote Desktop
- Enterprise DNS

---

## Linux Infrastructure

Configured Linux systems to operate as enterprise VPN routers.

### Implemented

- Ubuntu Server
- WireGuard
- Linux Routing
- IP Forwarding
- iptables
- SSH Administration
- VPN Monitoring
- Packet Capture

---

## Security

Implemented multiple enterprise security controls.

### Security Features

- WireGuard Encryption
- AWS Security Groups
- Azure Network Security Groups
- Windows Firewall
- Least Privilege Access
- Secure SSH Administration
- Secure RDP Administration
- Network Segmentation

---

# Technologies

## Cloud

- Amazon Web Services (AWS)
- Microsoft Azure
- Cloudflare

## Networking

- WireGuard
- TCP/IP
- Static Routing
- Linux Routing
- IP Forwarding
- iptables

## Operating Systems

- Ubuntu Server 26.04 LTS
- Windows Server 2025
- Windows 11 Enterprise
- Kali Linux

## Identity

- Active Directory Domain Services
- DNS
- Organizational Units
- Security Groups

## Administration

- PowerShell
- SSH
- VMware Workstation Pro

## Documentation

- Draw.io
- Git
- GitHub

---

# Validation

The infrastructure has been fully validated.

## Networking

- WireGuard Handshakes
- VPN Routing
- Static Routing
- Cross-Site Connectivity
- DNS Resolution
- End-to-End Communication

## Active Directory

- Domain Creation
- Organizational Units
- User Accounts
- Security Groups
- Computer Objects
- Domain Authentication

## Enterprise Clients

- Azure Windows 11 Domain Join
- On-Premises Windows 11 Domain Join
- Cross-Site Authentication
- Remote Desktop
- PowerShell Verification

Validation was performed using:

- ping
- tracert
- nslookup
- wg
- ip route
- tcpdump
- PowerShell
- Active Directory Users and Computers
- DNS Manager

---

# Repository Structure

```
configs/
documentation/
diagrams/
images/
scripts/
```

---

# Documentation

Complete technical documentation is included for every stage of the deployment.

- Project Outline
- Business Requirements
- Network Architecture
- IP Addressing
- AWS Infrastructure
- Azure Infrastructure
- On-Premises-Infrastructure
- VPN Architecture
- WireGuard Implementation
- CloudFlare
- Active Directory
- Security
- Testing & Validation
- Project Roadmap

---

# Skills Demonstrated

### Cloud Engineering

- Amazon Web Services
- Microsoft Azure
- Hybrid Cloud Architecture

### Infrastructure Engineering

- Enterprise Network Design
- Hybrid Networking
- Routing
- VPN Technologies
- Infrastructure Documentation

### Windows Administration

- Windows Server 2025
- Active Directory
- DNS
- Windows 11 Enterprise
- Enterprise Authentication

### Linux Administration

- Ubuntu Server
- WireGuard
- SSH
- Routing
- iptables
- Packet Forwarding
- Network Diagnostics

### Cybersecurity

- Secure Remote Administration
- Network Segmentation
- Defense in Depth
- VPN Encryption
- Firewall Configuration

---

# Project Outcome

This project successfully demonstrates the deployment of a secure enterprise hybrid cloud infrastructure integrating **AWS**, **Microsoft Azure**, and an **on-premises VMware environment** into a single production-style network.

The completed implementation includes:

- ✅ Enterprise hybrid cloud networking
- ✅ WireGuard hub-and-spoke VPN
- ✅ Cross-site routing
- ✅ Windows Server 2025 Active Directory
- ✅ Enterprise DNS
- ✅ Domain-joined Windows 11 workstations
- ✅ Cross-site authentication
- ✅ Secure remote administration
- ✅ Comprehensive technical documentation

This repository represents the completed infrastructure foundation for future infrastructure automation, enterprise application deployment, DevOps pipelines, and security engineering projects.

---

# Related Projects

This repository is the first component of a larger enterprise portfolio.

| Project | Purpose |
|---------|---------|
| **Enterprise Hybrid Cloud Platform** | Enterprise infrastructure foundation |
| **Hybrid Infrastructure Manager** *(Next Project)* | Python automation for infrastructure operations |
| **Enterprise Portfolio API** *(Future)* | FastAPI backend services |
| **Enterprise Portfolio Web** *(Future)* | React frontend application |

---

## Author

**Eric Robinson**

Infrastructure • Cloud • Networking • Windows Server • Linux • Cybersecurity