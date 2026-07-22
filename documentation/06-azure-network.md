# Azure Network 

## Overview

Microsoft Azure serves as the enterprise identity and infrastructure cloud for the Enterprise Hybrid Cloud Platform. Azure hosts Active Directory Domain Services, DNS, and future Windows infrastructure while maintaining secure connectivity to AWS and the on-premises environment through an encrypted WireGuard site-to-site VPN.

Current responsibilities include:

- Active Directory Domain Services
- Enterprise DNS
- Identity Management
- WireGuard VPN Gateway
- Hybrid Cloud Connectivity

Future responsibilities include:

- Windows File Server
- Internal Application Server
- Windows 11 Enterprise Client

---

# Azure Architecture

```
AWS WireGuard Hub
        │
Encrypted WireGuard Tunnel
        │
Azure WireGuard Gateway
        │
Azure VNET (10.1.0.0/16)
        │
──────────────────────────────
│ Public Subnet             │
│ Identity Services         │
│ Client Subnet             │
│ Infrastructure Subnet     │
──────────────────────────────
```

---

# Azure Virtual Network

| Property | Value |
|----------|-------|
| Virtual Network | 10.1.0.0/16 |
| Region | (Your Azure Region) |
| VPN Architecture | WireGuard Spoke |
| Connected Hub | AWS |

---

# Azure Subnets

## Public Subnet

```
10.1.1.0/24
```

Purpose

- WireGuard VPN Gateway
- Public-facing management access

Current Resources

- Ubuntu WireGuard Gateway

---

## Identity Services Subnet

```
10.1.2.0/24
```

Purpose

- Active Directory
- DNS
- Identity Services

Current Resources

- Windows Server 2025
- Active Directory Domain Services
- DNS Server

---

## Client Subnet

```
10.1.3.0/24
```

Purpose

Enterprise Workstations

Planned Resources

- Windows 11 Enterprise Client

---

## Infrastructure Services Subnet

```
10.1.4.0/24
```

Purpose

Enterprise Infrastructure

Planned Resources

- Windows File Server
- Internal Application Server

---

# Azure WireGuard Gateway

## Ubuntu Virtual Machine

Purpose

Provides secure encrypted connectivity between Azure, AWS, and the on-premises environment.

Interfaces

| Interface | Address |
|----------|---------|
| eth0 | 10.1.1.4 |
| wg0 | 172.16.100.2 |

---

# WireGuard Configuration

Azure operates as a spoke within the hub-and-spoke topology.

Connected Hub

AWS

```
172.16.100.1
```

Advertised Networks

```
10.1.0.0/16
```

Allowed Networks

```
10.0.0.0/16
10.2.0.0/16
172.16.100.1/32
172.16.100.3/32
```

Persistent Keepalive

```
25 Seconds
```

---

# Active Directory

## Windows Server 2025

Current Roles

- Active Directory Domain Services
- DNS Server

Configured

- Forest Created
- Domain Created
- Organizational Units
- Administrative Security Group
- Administrative User Account

Future Configuration

- Group Policy Objects
- Windows 11 Domain Join
- Additional Security Groups

---

# Routing

Azure advertises

```
10.1.0.0/16
```

Azure learns

```
10.0.0.0/16
10.2.0.0/16
```

Example Routing Table

```
10.0.0.0/16 dev wg0
10.2.0.0/16 dev wg0
```

Default Gateway

```
10.1.1.1
```

---

# Hybrid Connectivity

Azure currently has secure communication with

- AWS WireGuard Gateway
- AWS VPC
- AWS Application Network
- AWS Database Network
- On-Premises WireGuard Gateway
- Kali Linux Network

Traffic between environments is fully encrypted using WireGuard.

---

# Security

Azure Network Security Groups restrict access to:

Allowed

- SSH (22)
- WireGuard UDP (60031)
- RDP (3389) *(Administrative Access Only)*
- DNS (53)
- LDAP (389)
- LDAPS (636)
- Kerberos (88)

Restricted

- Internal Server Traffic
- Management Interfaces
- Administrative Services

---

# DNS Services

Current DNS Server

Windows Server 2025

Responsibilities

- Active Directory DNS
- Internal Name Resolution
- Hybrid Name Resolution (Future)

---

# Current Infrastructure

| Component | Status |
|-----------|--------|
| Azure Virtual Network | Complete |
| Public Subnet | Complete |
| Identity Services Subnet | Complete |
| Client Subnet | Complete |
| Infrastructure Subnet | Complete |
| Ubuntu WireGuard Gateway | Complete |
| WireGuard Site-to-Site VPN | Complete |
| Windows Server 2025 | Complete |
| Active Directory Domain Services | Complete |
| DNS Server | Complete |
| Administrative User | Complete |
| Administrative Security Group | Complete |
| Windows 11 Enterprise Client | Planned |
| File Server | Planned |
| Internal Application Server | Planned |

---

# Validation Performed

The following tests have been successfully completed.

✓ WireGuard handshake established

✓ Azure ↔ AWS connectivity

✓ Azure ↔ On-Premises connectivity

✓ ICMP testing successful

✓ Routing table verification

✓ WireGuard peer verification

✓ Active Directory installation

✓ DNS installation

✓ Domain creation

✓ Administrative user creation

✓ Administrative security group creation

---

# Future Enhancements

Planned improvements include

- Windows 11 domain join
- File Server deployment
- Internal Application Server deployment
- Group Policy implementation
- Azure Backup
- Azure Monitor
- Microsoft Defender for Servers
- Microsoft Entra Connect