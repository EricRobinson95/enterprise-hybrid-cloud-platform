# 10 - Active Directory

# Overview

Microsoft Active Directory Domain Services (AD DS) provides centralized identity, authentication, authorization, and DNS services for the Enterprise Hybrid Cloud Platform.

The domain controller is hosted in Microsoft Azure and serves as the centralized identity provider for Windows systems located in both Azure and the on-premises VMware environment. Secure authentication and directory services are delivered across the hybrid infrastructure through the encrypted WireGuard hub-and-spoke VPN.

The Active Directory deployment demonstrates enterprise identity management, organizational design, role-based administration, and secure cross-site authentication within a hybrid cloud environment.

---

# Business Purpose

Active Directory was implemented to provide a centralized enterprise identity platform capable of managing users, computers, security groups, authentication, authorization, and DNS from a single location.

The environment simulates a production enterprise where cloud and on-premises resources share the same authentication infrastructure while remaining connected through secure site-to-site VPN tunnels.

---

# Objectives

The Active Directory deployment provides:

- Centralized user authentication
- Centralized authorization
- Enterprise DNS
- Domain-joined Windows workstations
- Organizational Unit (OU) administration
- Security Group management
- Cross-site authentication
- Hybrid identity management
- Enterprise Windows administration
- Secure administrative delegation

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
          ┌────────────────────┴────────────────────┐
          │                                         │

 Azure Windows 11                        On-Premises Windows 11
 Administrative Workstation              Developer Workstation

           Authentication traverses the encrypted
           WireGuard Site-to-Site VPN Infrastructure
```

---

# Domain Controller

| Property | Value |
|----------|-------|
| Hostname | AZ-DC01 |
| Operating System | Windows Server 2025 |
| Environment | Microsoft Azure |
| Domain | corp.ericrobinsonjr.com |
| NetBIOS Name | CORP |
| Private IP Address | 10.1.2.4 |
| Primary Roles | Active Directory Domain Services, DNS |

---

# Installed Server Roles

Current Roles

- Active Directory Domain Services
- DNS Server

Planned Enterprise Roles

- File Services
- Group Policy Management
- Windows Server Backup
- Active Directory Certificate Services (Future)

---

# Active Directory Organizational Structure

The directory structure has been organized using enterprise administrative best practices.

```
corp.ericrobinsonjr.com

├── Administration
├── Employees
│   ├── Developers
│   ├── HR
│   ├── IT
│   └── Management
│
├── Groups
├── Security
├── Servers
├── Service Accounts
└── Workstations
```

This organizational structure separates users, computers, security objects, and infrastructure resources to simplify administration and future scalability.

---

# Enterprise User Accounts

The environment currently contains dedicated administrative and development identities.

## Administrative Account

```
Eric.Admin
```

Responsibilities

- Domain Administration
- Active Directory Administration
- Windows Server Administration
- Azure Infrastructure Administration
- Enterprise Infrastructure Management

---

## Developer Account

```
Eric.Developer
```

Responsibilities

- Software Development
- Application Development
- Source Code Management
- Enterprise Development Workstation
- Future React / FastAPI Development

Separating administrative and development accounts demonstrates the principle of least privilege by isolating privileged administrative activities from daily development tasks.

---

# Organizational Units

Current Organizational Units

```
Employees
│
├── Developers
├── HR
├── IT
└── Management

Servers

Workstations

Groups

Security

Service Accounts
```

The OU hierarchy supports delegated administration, Group Policy application, and future enterprise expansion.

---

# Security Groups

Current Security Groups include:

- Developers
- Management
- FileServer-Users
- RemoteDesktop-Users

Built-in Active Directory groups continue to provide domain administration and authentication services.

Security groups simplify permission management while supporting role-based access control (RBAC).

---

# Enterprise Computers

Current domain-joined systems include:

```
AZ-DC01

AZ-WIN11-01

ONPREM-WIN11-01
```

Windows workstations have been organized within the Workstations Organizational Unit according to enterprise administrative best practices.

---

# Enterprise DNS

Windows Server 2025 provides integrated Active Directory DNS services.

Responsibilities include:

- Active Directory-integrated DNS
- Internal name resolution
- Cross-site DNS resolution
- Domain controller discovery
- Hybrid enterprise name resolution

---

# Hybrid Authentication

Windows systems authenticate directly against the Azure-hosted domain controller regardless of physical location.

Supported authentication locations include:

- Microsoft Azure
- On-Premises VMware

Authentication traffic traverses the encrypted WireGuard VPN while Kerberos and DNS remain fully functional across the hybrid environment.

---

# Enterprise Workstations

## Azure Administrative Workstation

Purpose

- Infrastructure Administration
- Windows Server Administration
- Azure Administration
- Active Directory Management

Primary User

```
Eric.Admin
```

---

## On-Premises Developer Workstation

Purpose

- Software Development
- Source Code Management
- Future React Development
- Future FastAPI Development

Primary User

```
Eric.Developer
```

The developer workstation authenticates against the Azure-hosted domain controller through the WireGuard VPN.

---

# Authentication Workflow

```
Windows 11 Workstation

        │

DNS Resolution

        │

Domain Controller (AZ-DC01)

        │

Kerberos Authentication

        │

Domain Logon

        │

Enterprise Resource Access
```

---

# DNS Resolution Workflow

```
Windows Client

        │

DNS Query

        │

Windows Server 2025

        │

Active Directory DNS

        │

DNS Response
```

Cross-site DNS queries are securely transported across the WireGuard VPN.

---

# Validation

## Active Directory

Validated

- Active Directory Domain Services Installation
- Forest Creation
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

Validated

- Eric.Admin login
- Eric.Developer login
- Cross-site authentication

Verification

```powershell
whoami
```

Example Output

```
corp\eric.admin

corp\eric.developer
```

---

## DNS

Validated

- Internal DNS
- Domain Resolution
- Cross-site DNS Queries

Verification

```powershell
nslookup
```

---

## Domain Join

Validated

- Azure Windows 11
- On-Premises Windows 11

Verification

```powershell
hostname

systeminfo
```

---

# Current Infrastructure Status

| Component | Status |
|-----------|--------|
| Windows Server 2025 | Complete |
| Active Directory Domain Services | Complete |
| DNS Server | Complete |
| Enterprise Domain | Complete |
| Organizational Units | Complete |
| Enterprise Users | Complete |
| Security Groups | Complete |
| Computer Objects | Complete |
| Azure Administrative Workstation | Complete |
| On-Premises Developer Workstation | Complete |
| Cross-site Authentication | Complete |
| Cross-site DNS Resolution | Complete |

---

# Future Enhancements

Planned enterprise improvements include:

- Group Policy Objects (GPOs)
- Enterprise File Server
- Folder Redirection
- Roaming Profiles
- Active Directory Certificate Services (AD CS)
- Microsoft Entra ID Integration
- Microsoft Entra Connect
- Fine-Grained Password Policies
- Windows Admin Center
- Desired State Configuration (DSC)

---

# Lessons Learned

This deployment reinforced several enterprise concepts:

- Active Directory is dependent on reliable DNS.
- Hybrid authentication requires secure Layer 3 connectivity.
- Organizational Units simplify administration and policy management.
- Security Groups provide scalable role-based access control.
- VPN routing directly impacts Kerberos and DNS functionality.
- Administrative and development accounts should remain separate to support least-privilege administration.

---

# Summary

Microsoft Active Directory Domain Services provides the centralized identity platform for the Enterprise Hybrid Cloud Platform.

The completed deployment includes Windows Server 2025, Active Directory Domain Services, enterprise DNS, Organizational Units, security groups, dedicated administrative and development accounts, domain-joined Windows 11 workstations, and secure hybrid authentication across Microsoft Azure and an on-premises VMware environment.

Cross-site authentication, DNS resolution, and domain services have been successfully validated through the encrypted WireGuard hub-and-spoke VPN, demonstrating a production-style enterprise identity infrastructure capable of supporting future application hosting, infrastructure automation, and enterprise services.