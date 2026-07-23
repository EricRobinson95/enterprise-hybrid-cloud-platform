# 01 Project Overview

# Enterprise Hybrid Cloud Platform

## Overview

The Enterprise Hybrid Cloud Platform is a production-style infrastructure engineering project that demonstrates the design, implementation, and validation of a secure hybrid cloud environment spanning Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware infrastructure.

The environment integrates enterprise networking, hybrid cloud architecture, Active Directory Domain Services, enterprise DNS, Windows Server administration, Linux administration, and secure site-to-site connectivity into a single enterprise network.

All environments communicate through an encrypted WireGuard hub-and-spoke VPN, allowing cloud resources and on-premises systems to operate as a unified enterprise infrastructure while maintaining secure communication, centralized identity management, and network segmentation.

The project emphasizes practical infrastructure engineering rather than application development and demonstrates technologies commonly used by enterprise infrastructure, systems, and cloud engineers.

---

# Project Objectives

The objectives of this project were to:

- Design a production-style hybrid cloud architecture
- Build a secure WireGuard hub-and-spoke VPN
- Connect AWS, Azure, and an on-premises enterprise network
- Deploy centralized Active Directory Domain Services
- Implement enterprise DNS
- Configure enterprise routing across cloud providers
- Deploy domain-joined Windows 11 enterprise workstations
- Implement secure remote administration
- Validate hybrid authentication across cloud and on-premises environments
- Produce comprehensive infrastructure documentation

---

# Enterprise Architecture

The completed environment consists of three enterprise locations connected through encrypted site-to-site VPN tunnels.

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

      Azure Enterprise                 On-Premises Enterprise
          10.1.0.0/16                       10.2.0.0/16

 Windows Server 2025                 VMware Workstation
 Active Directory                    Ubuntu WireGuard Gateway
 DNS                                 Windows 11 Enterprise
 Windows 11 Enterprise               Kali Linux
```

---

# Infrastructure Overview

## AWS Production Environment

AWS serves as the central networking hub for the hybrid cloud.

Current infrastructure includes:

- Amazon Virtual Private Cloud (VPC)
- Public subnet
- Internet Gateway
- Route Tables
- Ubuntu WireGuard Gateway
- Linux IP Forwarding
- Static Routing
- Secure SSH Administration

Network

```
10.0.0.0/16
```

Responsibilities

- WireGuard VPN Hub
- Enterprise Routing
- Hybrid Cloud Connectivity
- Linux Routing
- Packet Forwarding

---

## Azure Enterprise Environment

Azure represents the organization's enterprise infrastructure.

Current infrastructure includes:

- Azure Virtual Network
- Ubuntu WireGuard Gateway
- Windows Server 2025
- Active Directory Domain Services
- DNS
- Organizational Units
- Security Groups
- Enterprise Users
- Azure Route Tables
- Windows 11 Enterprise Workstation

Network

```
10.1.0.0/16
```

Responsibilities

- Enterprise Identity
- Active Directory
- DNS
- Domain Authentication
- Enterprise Administration

---

## On-Premises Enterprise Environment

The on-premises environment simulates a corporate office connected securely to the cloud.

Current infrastructure includes:

- VMware Workstation Pro
- Ubuntu WireGuard Gateway
- Windows 11 Enterprise Workstation
- Kali Linux
- Enterprise Routing
- Secure SSH Administration

Network

```
10.2.0.0/16
```

Responsibilities

- Enterprise Client Simulation
- Security Operations Lab
- Hybrid Connectivity
- Windows Enterprise Administration

---

# Enterprise Networking

The networking infrastructure provides secure communication between all enterprise locations.

Implemented

- WireGuard Hub-and-Spoke VPN
- Site-to-Site VPN
- Static Routing
- Linux Packet Forwarding
- AWS Route Tables
- Azure User Defined Routes
- Windows Persistent Routes
- Cross-Site DNS Resolution
- Cross-Site Authentication

Validated communication

- AWS ↔ Azure
- AWS ↔ On-Premises
- Azure ↔ On-Premises

---

# Enterprise Identity

Identity services are centrally hosted within Microsoft Azure using Windows Server 2025 Active Directory Domain Services.

Implemented

- Active Directory Forest
- Enterprise Domain
- DNS
- Organizational Units
- Administrative Accounts
- Developer Accounts
- Security Groups
- Computer Objects
- Domain Authentication

Both enterprise Windows 11 workstations successfully authenticate against the Azure-hosted domain controller across the WireGuard VPN.

---

# Enterprise Workstations

The environment contains two enterprise Windows clients.

## Azure Enterprise Workstation

Purpose

- Enterprise Administration
- Remote Management
- Infrastructure Operations

Configured

- Windows 11 Enterprise
- Domain Joined
- Active Directory Authentication
- Enterprise DNS

---

## On-Premises Enterprise Workstation

Purpose

- Enterprise User Simulation
- Cross-Site Authentication
- Hybrid Connectivity Validation

Configured

- Windows 11 Enterprise
- Domain Joined
- Active Directory Authentication
- Enterprise DNS
- Remote Desktop

---

# Security

The infrastructure incorporates multiple enterprise security controls.

Implemented

- WireGuard VPN Encryption
- Network Segmentation
- AWS Security Groups
- Azure Network Security Groups
- Windows Firewall
- Linux Firewall
- Secure SSH Administration
- Secure Remote Desktop
- Active Directory Authentication
- Least Privilege Access

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
- Static Routing
- Linux Routing
- TCP/IP
- iptables

---

## Operating Systems

- Ubuntu Server 26.04 LTS
- Windows Server 2025
- Windows 11 Enterprise
- Kali Linux

---

## Identity

- Active Directory Domain Services
- DNS
- Organizational Units
- Security Groups

---

## Administration

- PowerShell
- SSH
- VMware Workstation Pro

---

## Documentation

- Draw.io
- Git
- GitHub

---

# Validation

The completed infrastructure has been successfully validated.

## Networking

Validated

- WireGuard Handshakes
- VPN Routing
- Static Routing
- Cross-Site Connectivity
- End-to-End Communication

Verification Tools

- ping
- tracert
- ip route
- wg
- tcpdump

---

## Active Directory

Validated

- Domain Creation
- Organizational Units
- Security Groups
- Administrative Accounts
- Developer Accounts
- Computer Objects
- Domain Authentication

---

## Windows Enterprise Clients

Validated

- Azure Windows 11 Domain Join
- On-Premises Windows 11 Domain Join
- Cross-Site Authentication
- Enterprise DNS
- PowerShell Verification
- Remote Desktop

---

# Skills Demonstrated

The project demonstrates practical experience with:

## Cloud Engineering

- Amazon Web Services
- Microsoft Azure
- Hybrid Cloud Architecture

## Infrastructure Engineering

- Enterprise Networking
- Routing
- VPN Technologies
- Infrastructure Documentation

## Windows Administration

- Windows Server 2025
- Active Directory
- DNS
- Windows 11 Enterprise

## Linux Administration

- Ubuntu Server
- WireGuard
- SSH
- Linux Routing
- Packet Forwarding
- iptables
- Network Diagnostics

## Cybersecurity

- Network Segmentation
- Secure Remote Administration
- Defense in Depth
- VPN Encryption
- Firewall Configuration

---

# Repository Documentation

Supporting documentation includes:

- Business Requirements
- Network Architecture
- IP Addressing
- AWS Infrastructure
- Azure Infrastructure
- VPN Architecture
- WireGuard Implementation
- Active Directory
- Security
- Testing & Validation
- Project Roadmap

---

# Project Outcome

The Enterprise Hybrid Cloud Platform successfully demonstrates the deployment of a secure enterprise hybrid cloud infrastructure integrating Amazon Web Services, Microsoft Azure, and an on-premises VMware environment into a unified production-style network.

The completed implementation includes enterprise networking, secure WireGuard connectivity, centralized Active Directory, enterprise DNS, hybrid routing, Windows Server administration, Linux administration, domain-joined Windows workstations, and comprehensive technical documentation.

This completed infrastructure serves as the foundation for future infrastructure automation, application deployment, DevOps, and security engineering projects while demonstrating practical enterprise infrastructure engineering skills.