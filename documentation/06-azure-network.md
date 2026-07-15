# 06 - Azure Network

# Enterprise Hybrid Cloud Platform - Azure Network Design

## Overview

Microsoft Azure provides the enterprise infrastructure services for the Enterprise Hybrid Cloud Platform. Unlike AWS, which hosts the production application, Azure is responsible for identity management, Windows administration, file services, monitoring, backup, and enterprise infrastructure.

The Azure environment is deployed within a dedicated Virtual Network (VNet) that separates identity, management, and infrastructure services into individual private subnets. Azure also securely connects to AWS through a WireGuard site-to-site VPN.

---

# Design Objectives

The Azure environment has been designed to:

- Provide centralized identity management
- Host Windows Server infrastructure
- Deliver Active Directory Domain Services (AD DS)
- Provide internal DNS services
- Host enterprise file services
- Support secure remote administration
- Provide monitoring and backup capabilities
- Integrate securely with AWS and the on-premises environment
- Support Infrastructure as Code (Terraform)

---

# Azure Virtual Network (VNet)

All Azure resources are deployed inside a dedicated Azure Virtual Network.

## VNet Configuration

| Property | Value |
|----------|-------|
| Network | Azure Virtual Network |
| CIDR Block | 10.1.0.0/16 |

Using a /16 network allows future subnet expansion while maintaining a structured enterprise addressing scheme.

---

# Subnet Design

The Azure VNet is divided into three primary private subnets.

## Identity Services Subnet

| CIDR | 10.1.1.0/24 |

Purpose:

- Centralized authentication
- Identity management
- Internal DNS

Resources:

- Windows Server 2025
- Active Directory Domain Services (AD DS)
- DNS Server

This subnet hosts the Domain Controller responsible for authenticating users and managing enterprise identity.

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

Azure Bastion provides secure browser-based RDP and SSH access without exposing management systems directly to the Internet.

---

## Infrastructure Services Subnet

| CIDR | 10.1.3.0/24 |

Purpose:

- Shared infrastructure services
- Monitoring
- Backup
- File storage

Resources:

- Windows File Server
- Azure Monitor
- Azure Backup

These services support the overall enterprise infrastructure while remaining isolated from identity and management resources.

---

# Windows Server

Windows Server 2025 serves as the primary enterprise infrastructure server.

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

Running Windows Server instead of fully managed directory services provides valuable hands-on experience with enterprise administration.

---

# Active Directory Domain Services

Active Directory Domain Services (AD DS) provides centralized authentication and authorization.

Capabilities include:

- User authentication
- Computer authentication
- Group Policy management
- Organizational Unit administration
- Security group management
- Domain administration

Azure acts as the identity provider for the hybrid cloud platform.

---

# DNS Services

The integrated DNS Server provides internal name resolution for Azure resources and hybrid cloud services.

Responsibilities include:

- Internal hostname resolution
- Active Directory integration
- Service discovery
- Domain Controller location

DNS is integrated directly with Active Directory.

---

# Azure Bastion

Azure Bastion provides secure remote administration.

Benefits include:

- Secure RDP
- Secure SSH
- Browser-based access
- No public IP addresses required on management VMs
- Reduced attack surface

---

# Windows File Server

The Windows File Server provides centralized storage for enterprise resources.

Planned services include:

- Department shared folders
- User home directories
- NTFS permissions
- SMB file sharing

---

# Azure Monitor

Azure Monitor provides operational visibility throughout the Azure environment.

Monitoring includes:

- Virtual machine health
- Performance metrics
- Event logs
- Resource utilization
- Alerting

---

# Azure Backup

Azure Backup protects enterprise infrastructure.

Capabilities include:

- Virtual machine backups
- File recovery
- Scheduled backups
- Long-term retention
- Disaster recovery support

---

# VPN Connectivity

Azure securely connects to AWS through a dedicated WireGuard VPN tunnel.

A dedicated Ubuntu Virtual Machine running WireGuard functions as the Azure VPN Gateway.

Responsibilities include:

- Encrypting VPN traffic
- Decrypting VPN traffic
- Routing Azure traffic toward AWS
- Providing secure hybrid cloud connectivity

Azure does not communicate directly with the on-premises environment. Instead, all inter-site communication is routed through AWS using the hub-and-spoke VPN topology.

Detailed VPN implementation is documented in **07-vpn-architecture.md**.

---

# Hybrid Cloud Integration

Azure integrates with:

- AWS Virtual Private Cloud (10.0.0.0/16)
- On-Premises Network (10.2.0.0/16)
- WireGuard VPN
- Cloudflare (indirectly through AWS)

Azure provides enterprise services that support workloads hosted across the hybrid cloud platform.

---

# Routing

Traffic routing occurs in multiple stages.

1. Azure Route Tables determine packet forwarding.
2. Linux routing on the Ubuntu VPN Gateway selects the correct interface.
3. WireGuard encrypts traffic and forwards it to AWS.
4. AWS forwards traffic to the appropriate destination when required.

This layered routing approach separates cloud networking from enterprise services.

---

# Security Model

The Azure environment follows a defense-in-depth security strategy.

Security is achieved through:

- Private subnets
- Network segmentation
- Centralized authentication
- Dedicated VPN gateway
- Secure management access
- Least privilege administration

Future security enhancements include:

- Network Security Groups (NSGs)
- Azure Firewall
- Microsoft Defender for Cloud
- Azure Key Vault
- Conditional Access Policies

---

# Future Improvements

Future enhancements include:

- Azure VPN Gateway
- GatewaySubnet
- Network Security Groups
- Azure Firewall
- Microsoft Defender for Cloud
- Azure Automation
- Microsoft Entra ID integration
- Terraform deployment
- GitHub Actions CI/CD

---

# Design Decisions

### Why a dedicated Azure VNet?

A dedicated Virtual Network provides network isolation while supporting secure hybrid cloud connectivity.

### Why Windows Server?

Running Windows Server allows hands-on experience with Active Directory, DNS, Group Policy, file services, and enterprise administration.

### Why separate subnets?

Separating identity, management, and infrastructure services improves security, simplifies administration, and follows enterprise networking best practices.

### Why Azure Bastion?

Azure Bastion provides secure administrative access without exposing management virtual machines directly to the Internet.

### Why a dedicated WireGuard VM?

Azure does not provide a managed WireGuard service. A dedicated Ubuntu Virtual Machine running WireGuard provides secure VPN connectivity while keeping networking functions separate from enterprise services.

---

# Summary

Azure serves as the enterprise infrastructure platform for the Enterprise Hybrid Cloud Platform. By combining Windows Server 2025, Active Directory Domain Services, DNS, Azure Bastion, Windows File Server, Azure Monitor, Azure Backup, and a dedicated WireGuard VPN Gateway within a segmented Virtual Network, Azure provides secure identity management, enterprise administration, and hybrid cloud connectivity while supporting the AWS-hosted production application.