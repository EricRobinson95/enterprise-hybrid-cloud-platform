# Active Directory

## Overview

Microsoft Active Directory Domain Services (AD DS) provides centralized identity and access management for the Enterprise Hybrid Cloud Platform.

The domain controller is hosted within Microsoft Azure and serves as the primary authentication source for cloud and on-premises Windows systems connected through the WireGuard hub-and-spoke VPN.

The Active Directory environment enables centralized management of users, groups, computers, authentication, DNS, and future Group Policy Objects (GPOs).

---

# Objectives

The Active Directory deployment was designed to provide:

- Centralized user authentication
- Centralized authorization
- Enterprise DNS
- Domain-joined Windows devices
- Security Group management
- Organizational Unit (OU) management
- Hybrid cloud identity services
- Secure authentication across VPN-connected environments

---

# Architecture

```
                   Azure Virtual Network
                     10.1.0.0/16
                          │
                          │
                Windows Server 2025
               Active Directory Domain
                          │
          ┌───────────────┴───────────────┐
          │                               │
     Azure Windows 11          On-Premises Windows 11
     (Future)                  (Future)
```

All authentication requests traverse the WireGuard VPN when originating from the on-premises environment.

---

# Domain Controller

| Property | Value |
|----------|-------|
| Operating System | Windows Server 2025 |
| Roles Installed | Active Directory Domain Services, DNS |
| Environment | Microsoft Azure |
| Network | 10.1.0.0/16 |

---

# Installed Roles

Current roles include:

- Active Directory Domain Services (AD DS)
- DNS Server

Future planned roles:

- File Server
- Group Policy Management
- Windows Server Backup

---

# Active Directory Structure

Current Active Directory objects include:

- Domain
- Organizational Units (OUs)
- User Accounts
- Security Groups

The structure is designed to support future expansion as additional servers and workstations are added to the environment.

---

# Organizational Units

Current Organizational Units include:

- Users
- Groups

Future Organizational Units may include:

- Servers
- Workstations
- Service Accounts
- IT Administration

---

# User Management

Current implementation includes:

- Administrative user account
- Standard user account(s)
- Password policy configuration

Users are centrally managed through Active Directory.

---

# Security Groups

Security Groups are used to simplify permission management.

Current implementation includes:

- Administrative Group

Future groups may include:

- Help Desk
- Network Administrators
- Server Administrators
- Domain Users
- Security Operations

---

# DNS

The Windows Server 2025 domain controller provides DNS services for the enterprise environment.

Current responsibilities include:

- Active Directory DNS
- Internal name resolution

Future responsibilities include:

- Hybrid name resolution
- Conditional Forwarders
- DNS forwarding to cloud services

---

# Hybrid Connectivity

The domain controller is reachable through the WireGuard VPN.

Connected environments include:

- Azure
- AWS
- On-Premises VMware

This architecture enables future Windows clients located on-premises to authenticate against the Azure-hosted domain controller securely.

---

# Planned Windows Clients

## Azure

Planned

- Windows 11 Enterprise
- Domain Joined

Purpose

- Administrative workstation
- Group Policy testing
- Enterprise management

---

## On-Premises

Planned

- Windows 11 Enterprise
- Domain Joined

Purpose

- Hybrid authentication
- Remote user simulation
- VPN authentication testing

---

# Planned Group Policy

Future Group Policy Objects (GPOs) include:

- Password Policy
- Account Lockout Policy
- Windows Firewall Configuration
- Desktop Security
- Windows Updates
- Software Restrictions
- Drive Mapping
- Security Baselines

---

# Authentication Flow

```
Windows Client

↓

DNS Resolution

↓

Domain Controller

↓

Kerberos Authentication

↓

Access Granted
```

Clients located on-premises communicate securely with the domain controller through the encrypted WireGuard VPN.

---

# Validation

The following tasks have been successfully completed:

✓ Windows Server 2025 deployed

✓ Active Directory Domain Services installed

✓ DNS Server installed

✓ Domain created

✓ Organizational Units created

✓ User account created

✓ Security Group created

✓ User assigned to Security Group

---

# Remaining Tasks

The following work remains:

- Deploy Azure Windows 11 Enterprise client
- Join Azure Windows 11 to the domain
- Deploy On-Premises Windows 11 Enterprise client
- Join On-Premises Windows 11 to the domain
- Configure Group Policy Objects
- Verify cross-site authentication
- Validate DNS resolution across the VPN
- Test Kerberos authentication
- Test remote administration

---

# Future Enhancements

Future improvements include:

- Active Directory Certificate Services (AD CS)
- Microsoft Entra ID integration
- Microsoft Entra Connect
- Multi-Domain Controller deployment
- Read-Only Domain Controller (RODC)
- Fine-Grained Password Policies
- Azure Backup
- Windows Admin Center
- Desired State Configuration (DSC)

---

# Current Status

| Component | Status |
|-----------|--------|
| Windows Server 2025 | Complete |
| Active Directory Domain Services | Complete |
| DNS Server | Complete |
| Domain | Complete |
| Organizational Units | Complete |
| User Accounts | Complete |
| Security Groups | Complete |
| Administrative User | Complete |
| Azure Windows 11 Client | Planned |
| On-Premises Windows 11 Client | Planned |
| Domain Join Validation | Planned |
| Group Policy | Planned |

---

# Summary

Active Directory provides centralized identity and authentication services for the Enterprise Hybrid Cloud Platform. Hosting the domain controller within Microsoft Azure allows both cloud and on-premises resources to authenticate against a single identity provider over the secure WireGuard VPN.

With the networking infrastructure complete, the next phase of the project is to deploy Windows 11 clients, join them to the domain, validate Group Policy, and demonstrate hybrid authentication across cloud and on-premises environments.