# 06 - Azure Networking

# Overview

Microsoft Azure provides the corporate IT environment for the Enterprise Hybrid Cloud Platform.

The Azure environment hosts enterprise identity services, developer workstations, file services, and internal applications while maintaining secure connectivity to AWS through a WireGuard site-to-site VPN.

Unlike AWS, which hosts production workloads, Azure is dedicated to corporate infrastructure and enterprise administration.

---

# Azure Responsibilities

The Azure environment is responsible for:

- Enterprise Identity
- Active Directory Domain Services
- DNS
- Group Policy
- Windows File Services
- Internal Applications
- Developer Workstations
- Secure Connectivity to AWS

---

# Azure Network Architecture

```
                   Azure Virtual Network
                      10.1.0.0/16
                            │
        ┌───────────────────┼────────────────────┐
        │                   │                    │
        ▼                   ▼                    ▼

 Public Subnet      Identity Services      Developer Subnet
 10.1.1.0/24          10.1.2.0/24           10.1.3.0/24

 Ubuntu WG          Windows Server        Windows 11
 Gateway            Active Directory      Developer VM
                    DNS

                            │
                            ▼

                Infrastructure Services
                     10.1.4.0/24

              Windows File Server
              Internal Application Server
```

---

# Azure Virtual Network (VNet)

| Setting | Value |
|----------|-------|
| Address Space | 10.1.0.0/16 |
| DNS | Active Directory DNS |
| Region | Azure Deployment Region |

The Virtual Network provides secure communication between all Azure resources.

---

# Subnet Design

## Public Subnet

Network

```
10.1.1.0/24
```

Purpose

- Ubuntu WireGuard Gateway

This subnet provides secure connectivity between Azure and AWS.

---

## Identity Services Subnet

Network

```
10.1.2.0/24
```

Purpose

- Windows Server 2025
- Active Directory Domain Services
- DNS

This subnet hosts the organization's enterprise identity infrastructure.

---

## Developer Subnet

Network

```
10.1.3.0/24
```

Purpose

- Windows 11 Developer Workstation

The workstation is joined to the Active Directory domain and provides a standardized development environment.

---

## Infrastructure Services Subnet

Network

```
10.1.4.0/24
```

Purpose

- Windows File Server
- Internal Application Server

This subnet hosts internal business services used by employees.

---

# Planned Infrastructure

| Resource | Subnet | Planned IP |
|-----------|--------|------------|
| Ubuntu WireGuard Gateway | Public | 10.1.1.4 |
| Windows Server 2025 | Identity | 10.1.2.10 |
| Windows 11 Developer VM | Developer | 10.1.3.10 |
| Windows File Server | Infrastructure | 10.1.4.10 |
| Internal Application Server | Infrastructure | 10.1.4.20 |

---

# Network Security Groups (NSGs)

Each subnet is protected using Azure Network Security Groups.

Example rules include:

| Protocol | Port | Purpose |
|----------|------|----------|
| RDP | 3389 | Windows Administration |
| SSH | 22 | Ubuntu Administration |
| DNS | 53 | Active Directory DNS |
| WireGuard | UDP 51820 | VPN Connectivity |

Additional rules will be implemented following the principle of least privilege.

---

# WireGuard Gateway

Azure connects to AWS through an Ubuntu-based WireGuard gateway.

Tunnel Network

```
172.16.100.2/24
```

Peer

```
AWS WireGuard Hub
172.16.100.1
```

Advertised Network

```
10.1.0.0/16
```

Responsibilities

- VPN Connectivity
- Secure Routing
- Packet Forwarding
- Encrypted Communication

---

# Active Directory Services

Windows Server 2025 will provide:

- Active Directory Domain Services
- DNS
- Organizational Units
- Security Groups
- User Authentication
- Group Policy

Example users:

```
Eric.Admin

John.Developer
```

---

# Developer Environment

The Windows 11 Developer Workstation will provide a managed enterprise development environment.

Installed software will include:

- Visual Studio Code
- Git
- GitHub Desktop (Optional)
- Python
- Node.js
- Docker Desktop
- AWS CLI
- Azure CLI

Developers will connect remotely using Remote Desktop Protocol (RDP).

---

# File Services

Windows File Server will provide:

- Shared Project Files
- Department Shares
- Backup Storage
- NTFS Permissions

Access will be controlled through Active Directory security groups.

---

# Internal Application Server

The Internal Application Server may host:

- Internal Documentation
- Package Repository
- Development Utilities
- Administrative Tools

This server is intended for internal corporate services only.

---

# Routing

Azure advertises:

```
10.1.0.0/16
```

Traffic destined for AWS traverses the WireGuard tunnel.

Example routes

```
10.0.0.0/16 dev wg0

172.16.100.0/24 dev wg0
```

---

# Identity Architecture

Azure provides centralized identity management.

Responsibilities include:

- User Authentication
- Computer Authentication
- Group Policy
- DNS
- Enterprise Administration

AWS IAM remains responsible for authorizing access to AWS resources and is separate from Active Directory.

---

# Validation

The following components have been validated.

## Completed

- Azure Virtual Network
- Ubuntu WireGuard Gateway
- AWS VPN Connectivity
- Static Routing
- Linux IP Forwarding

---

## Planned

- Windows Server 2025
- Active Directory
- DNS
- Group Policy
- Windows File Server
- Windows 11 Developer Workstation
- Domain Join
- Remote Desktop

---

# Current Status

## Operational

- Azure Networking
- WireGuard VPN
- AWS Connectivity
- Static Routing

## In Progress

- Enterprise Identity Infrastructure

---

# Future Enhancements

Future improvements may include:

- Microsoft Entra ID
- Azure Bastion
- Azure Backup
- Azure Monitor
- Azure Update Manager
- Azure Key Vault
- Azure Automation
- Azure Site Recovery

---

# Summary

The Azure environment serves as the corporate IT platform for the Enterprise Hybrid Cloud Platform.

It provides enterprise identity services, developer workstations, file services, and internal infrastructure while maintaining secure connectivity to AWS through a WireGuard site-to-site VPN. Azure supports employee authentication, software development, and corporate administration, complementing the production workloads hosted in AWS.