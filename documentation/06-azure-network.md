# 06 - Azure Network

# Enterprise Hybrid Cloud Platform - Azure Network Design

## Overview

Microsoft Azure provides the enterprise infrastructure services for the Enterprise Hybrid Cloud Platform. Unlike AWS, which hosts the production application, Azure is responsible for identity management, Windows administration, file services, monitoring, and infrastructure management.

The Azure environment is designed around a dedicated Virtual Network (VNet) that isolates enterprise services into separate private subnets. This approach improves security, simplifies administration, and provides a scalable foundation for future growth.

---

# Design Objectives

The Azure environment has been designed to:

- Provide centralized identity management
- Host Windows Server infrastructure
- Deliver Active Directory Domain Services
- Provide internal DNS services
- Host enterprise file services
- Support secure remote administration
- Provide monitoring and backup capabilities
- Integrate securely with AWS and the on-premises environment

---

# Azure Virtual Network (VNet)

All Azure resources are deployed within a dedicated Virtual Network.

## VNet Configuration

| Property | Value |
|----------|-------|
| Network | Azure Virtual Network |
| CIDR Block | 10.1.0.0/16 |

The /16 address space provides sufficient room for future expansion while maintaining clear separation from AWS and the on-premises network.

---

# Subnet Design

The Azure VNet is divided into three private subnets based on their operational role.

---

## Identity Services Subnet

| CIDR | 10.1.1.0/24 |

Purpose:

- Centralized authentication
- Active Directory
- Internal DNS

Resources:

- Windows Server 2025
- Active Directory Domain Services (AD DS)
- DNS Server

The Windows Server virtual machine functions as the enterprise Domain Controller, providing authentication, directory services, and internal name resolution.

---

## Management Subnet

| CIDR | 10.1.2.0/24 |

Purpose:

- Secure administration
- Remote management
- Infrastructure access

Resources:

- Management Virtual Machine
- Azure Bastion

Azure Bastion enables secure RDP and SSH access without exposing management systems directly to the Internet.

---

## Infrastructure Services Subnet

| CIDR | 10.1.3.0/24 |

Purpose:

- Enterprise infrastructure services
- Monitoring
- Backup

Resources:

- Windows File Server
- Azure Monitor
- Azure Backup

This subnet hosts supporting services that are used throughout the enterprise environment.

---

# Windows Server

Windows Server 2025 is the primary infrastructure server within Azure.

Installed Roles:

- Active Directory Domain Services
- DNS Server
- File Services

Responsibilities include:

- User authentication
- Computer authentication
- Group Policy
- Organizational Units
- Shared folders
- Internal DNS resolution

---

# Active Directory Domain Services

Active Directory Domain Services (AD DS) provides centralized identity management for the enterprise.

Capabilities include:

- User authentication
- Computer authentication
- Group Policy management
- Organizational Unit management
- Security group management
- Domain administration

This environment mirrors many of the services commonly deployed within enterprise Windows networks.

---

# DNS Services

The internal DNS server provides name resolution for Azure resources and future hybrid cloud services.

Responsibilities include:

- Internal hostname resolution
- Active Directory integration
- Service discovery
- Domain controller location

DNS is integrated directly with Active Directory.

---

# Azure Bastion

Azure Bastion provides secure administrative access to Azure virtual machines.

Benefits include:

- Secure RDP
- Secure SSH
- No public IP addresses required on management servers
- Browser-based administration
- Reduced attack surface

---

# Windows File Server

The Windows File Server provides centralized storage for enterprise resources.

Planned services include:

- Department shared folders
- User home directories
- File permissions
- NTFS security
- SMB file sharing

---

# Azure Monitor

Azure Monitor provides operational visibility across the Azure environment.

Monitoring includes:

- Virtual machine health
- Performance metrics
- System logs
- Resource utilization
- Alerting

---

# Azure Backup

Azure Backup protects critical infrastructure.

Capabilities include:

- Virtual machine backups
- File recovery
- Scheduled backups
- Long-term retention
- Disaster recovery support

---

# Security Model

The Azure environment follows a layered security approach.

Security is achieved through:

- Private subnets
- Centralized authentication
- Secure management access
- Role-based administration
- Network isolation
- Least privilege access

Future security enhancements include:

- Network Security Groups (NSGs)
- Azure Firewall
- Microsoft Defender for Cloud
- Azure Key Vault
- Conditional Access policies

---

# Hybrid Cloud Integration

The Azure environment is designed to integrate with:

- AWS Virtual Private Cloud (10.0.0.0/16)
- On-Premises Network
- WireGuard VPN
- Cloudflare

Hybrid connectivity will allow enterprise services such as Active Directory and DNS to support workloads hosted across multiple environments.

---

# Design Decisions

Several architectural decisions influenced the Azure network design.

### Why a dedicated Azure VNet?

Using a dedicated VNet provides complete network isolation while allowing secure hybrid connectivity with AWS and the on-premises environment.

### Why Windows Server?

Running Windows Server allows hands-on administration of Active Directory, DNS, Group Policy, and enterprise file services, providing valuable real-world experience.

### Why separate subnets?

Separating identity, management, and infrastructure services reduces the attack surface and simplifies administration.

### Why Azure Bastion?

Azure Bastion enables secure remote management without assigning public IP addresses to administrative virtual machines.

---

# Future Improvements

The Azure environment will continue to evolve throughout the project.

Planned enhancements include:

- Azure VPN Gateway
- GatewaySubnet
- Network Security Groups
- Azure Firewall
- Microsoft Defender for Cloud
- Azure Key Vault
- Terraform deployment
- Azure Automation
- Microsoft Entra ID integration

---

# Summary

The Azure environment serves as the enterprise management platform for the Enterprise Hybrid Cloud Platform. By combining Windows Server 2025, Active Directory Domain Services, DNS, Azure Bastion, Windows File Server, Azure Monitor, and Azure Backup within a segmented Virtual Network, the design provides a secure and scalable foundation for identity management, infrastructure administration, and hybrid cloud operations.