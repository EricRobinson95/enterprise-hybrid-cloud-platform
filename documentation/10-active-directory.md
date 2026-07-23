# 10 - Active Directory

# Overview

Microsoft Active Directory Domain Services (AD DS) provides centralized identity and access management for the Enterprise Hybrid Cloud Platform.

The domain controller is hosted within Microsoft Azure and serves as the centralized authentication source for enterprise Windows systems located in both Azure and the on-premises VMware environment.

The Active Directory environment provides centralized authentication, authorization, enterprise DNS, Organizational Unit (OU) management, security groups, and Windows domain services across the hybrid cloud using the WireGuard site-to-site VPN.

---

# Objectives

The Active Directory deployment provides:

- Centralized user authentication
- Centralized authorization
- Enterprise DNS
- Domain-joined Windows workstations
- Organizational Unit management
- Security Group management
- Cross-site authentication
- Hybrid identity management
- Enterprise Windows administration

---

# Enterprise Identity Architecture

```
                    Azure Virtual Network
                       10.1.0.0/16
                              │
                     Windows Server 2025
                 Active Directory Domain Services
                              │
              corp.ericrobinsonjr.com
                              │
             ┌────────────────┴────────────────┐
             │                                 │

 Azure Windows 11                 On-Premises Windows 11
 Domain Joined                    Domain Joined

           Authentication traverses the
         encrypted WireGuard Site-to-Site VPN
```

---

# Domain Controller

| Property | Value |
|----------|-------|
| Operating System | Windows Server 2025 |
| Hostname | AZ-DC01 |
| Domain | corp.ericrobinsonjr.com |
| NetBIOS Name | CORP |
| Environment | Microsoft Azure |
| IP Address | 10.1.2.4 |

---

# Installed Roles

Current Roles

- Active Directory Domain Services
- DNS Server

Future Roles

- File Services
- Group Policy Management
- Windows Server Backup

---

# Active Directory Structure

The Active Directory environment currently contains:

- Domain
- Organizational Units
- Enterprise Users
- Security Groups
- Computer Objects

The directory structure has been organized using enterprise best practices to simplify administration and future expansion.

---

# Organizational Units

Current Organizational Units

```
Employees
├── Developers
├── HR
├── IT
└── Management

Servers

Workstations

Groups

Service Accounts
```

This structure separates users, computers, and infrastructure resources into dedicated administrative containers.

---

# Enterprise Users

Current implementation includes:

- Azure Administrator
- Enterprise Developer Account

Example Accounts

```
azureadmin

EricRobinson95
```

Enterprise users authenticate directly against Active Directory.

---

# Security Groups

Current implementation includes:

- Administrative Security Group

Future groups include:

- Help Desk
- Network Administrators
- Server Administrators
- Developers
- Security Operations

Security Groups simplify permission management across enterprise resources.

---

# Computer Objects

Current computer objects include:

```
AZ-DC01

ONPREM-WIN11-01
```

The on-premises Windows 11 workstation has successfully joined the Active Directory domain and has been moved into the dedicated **Workstations** Organizational Unit following enterprise best practices.

---

# Enterprise DNS

Windows Server 2025 provides centralized DNS services.

Responsibilities

- Active Directory DNS
- Internal Name Resolution
- Cross-site DNS Resolution

Current validation includes:

- Domain lookups
- Forward lookups
- Client authentication
- Name resolution across the WireGuard VPN

---

# Hybrid Authentication

The Active Directory domain controller authenticates Windows systems located in multiple enterprise environments.

Connected Locations

- Azure
- On-Premises

Authentication is securely transported across the encrypted WireGuard site-to-site VPN.

---

# Windows Enterprise Workstations

## Azure

Current

- Windows 11 Enterprise
- Domain Joined

Purpose

- Administrative Workstation
- Enterprise Management

---

## On-Premises

Current

- Windows 11 Enterprise
- Domain Joined

Purpose

- Developer Workstation
- Hybrid Authentication
- Enterprise Resource Access

The workstation authenticates against the Azure-hosted domain controller through the WireGuard VPN.

---

# Authentication Process

```
Windows 11 Workstation

↓

DNS Resolution

↓

Domain Controller (AZ-DC01)

↓

Kerberos Authentication

↓

Domain Login

↓

Enterprise Resource Access
```

---

# Enterprise DNS Flow

```
Windows 11

↓

DNS Query

↓

Windows Server 2025

↓

Active Directory DNS

↓

Response Returned
```

The on-premises workstation resolves enterprise DNS records through the encrypted VPN.

---

# Validation

The Active Directory deployment has been fully validated.

## Infrastructure

Verified

- Windows Server 2025
- Active Directory Domain Services
- DNS Installation

---

## Directory Services

Verified

- Domain Creation
- Organizational Units
- Enterprise Users
- Security Groups
- Computer Objects

Verification

```powershell
Get-ADUser

Get-ADComputer
```

---

## Authentication

Verified

- Azure Administrator Login
- Developer Login
- Cross-site Domain Authentication

Verification

```powershell
whoami
```

Example

```
corp\azureadmin

corp\ericrobinson95
```

---

## DNS

Verified

- Internal DNS
- Cross-site Name Resolution

Verification

```powershell
nslookup
```

---

## Domain Join

Verified

- Azure Windows 11
- On-Premises Windows 11

Verification

```powershell
systeminfo

hostname
```

---

# Current Infrastructure

| Component | Status |
|-----------|--------|
| Windows Server 2025 | Complete |
| Active Directory Domain Services | Complete |
| DNS Server | Complete |
| Domain | Complete |
| Organizational Units | Complete |
| Enterprise Users | Complete |
| Security Groups | Complete |
| Computer Objects | Complete |
| Azure Windows 11 Domain Join | Complete |
| On-Premises Windows 11 Domain Join | Complete |
| Cross-site Authentication | Complete |
| Cross-site DNS Resolution | Complete |

---

# Future Enhancements

Planned improvements include:

- Group Policy Objects (GPOs)
- Windows File Server
- Roaming Profiles
- Folder Redirection
- Microsoft Entra ID Integration
- Microsoft Entra Connect
- Active Directory Certificate Services (AD CS)
- Fine-Grained Password Policies
- Read-Only Domain Controller (RODC)
- Windows Admin Center
- Desired State Configuration (DSC)

---

# Lessons Learned

The Active Directory deployment reinforced several enterprise concepts:

- Active Directory depends on reliable DNS.
- Secure routing is required for cross-site authentication.
- Domain-joined workstations should be organized into dedicated Organizational Units.
- Security Groups simplify permission management.
- Cross-site authentication requires functional VPN connectivity and routing.
- Enterprise identity services can be successfully hosted in Azure while authenticating on-premises systems.

---

# Summary

Microsoft Active Directory Domain Services provides centralized identity management for the Enterprise Hybrid Cloud Platform.

The completed deployment includes Windows Server 2025, Active Directory Domain Services, enterprise DNS, Organizational Units, security groups, enterprise user accounts, computer objects, and domain-joined Windows 11 workstations located in both Azure and the on-premises enterprise environment.

Cross-site authentication and DNS resolution have been successfully validated across the encrypted WireGuard site-to-site VPN, providing a production-style enterprise identity platform that supports centralized administration, hybrid cloud networking, and future enterprise services.