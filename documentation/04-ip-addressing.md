# 04 - IP Addressing

# Overview

This document describes the IP addressing scheme used throughout the Enterprise Hybrid Cloud Platform. The addressing plan provides unique, non-overlapping private networks for Amazon Web Services (AWS), Microsoft Azure, and the WireGuard VPN tunnel.

Using separate address spaces allows secure routed communication between cloud providers without requiring Network Address Translation (NAT).

---

# Objectives

- Create a scalable IP addressing plan.
- Prevent overlapping private networks.
- Separate cloud infrastructure into logical networks.
- Provide dedicated addressing for the WireGuard tunnel.
- Support future enterprise services.

---

# Network Summary

| Network | CIDR | Purpose |
|----------|------|---------|
| AWS VPC | 10.0.0.0/16 | AWS Private Network |
| Azure VNet | 10.1.0.0/16 | Azure Private Network |
| WireGuard Tunnel | 172.168.100.0/24 | VPN Tunnel Network |

---

# AWS Addressing

## Virtual Private Cloud

| Setting | Value |
|----------|-------|
| CIDR Block | 10.0.0.0/16 |

---

## Public Subnet

| Setting | Value |
|----------|-------|
| CIDR Block | 10.0.1.0/24 |
| Purpose | Internet-facing infrastructure |

---

## WireGuard Gateway

| Resource | Address |
|----------|---------|
| EC2 Private IP | 10.0.1.40 |
| WireGuard Interface | 172.168.100.1/24 |

---

# Azure Addressing

## Virtual Network

| Setting | Value |
|----------|-------|
| Address Space | 10.1.0.0/16 |

---

## Public Subnet

| Setting | Value |
|----------|-------|
| CIDR Block | 10.1.1.0/24 |
| Purpose | Internet-facing infrastructure |

---

## WireGuard Gateway

| Resource | Address |
|----------|---------|
| VM Private IP | 10.1.1.4 |
| WireGuard Interface | 172.168.100.2/24 |

---

# WireGuard Tunnel Network

The WireGuard VPN uses its own dedicated subnet.

| Resource | Address |
|----------|---------|
| Tunnel Network | 172.168.100.0/24 |
| AWS Gateway | 172.168.100.1 |
| Azure Gateway | 172.168.100.2 |

The tunnel network is used exclusively for VPN communication between the AWS and Azure gateways.

---

# Address Allocation Diagram

```text
AWS
──────────────────────────────────

VPC
10.0.0.0/16

└── Public Subnet
    10.0.1.0/24

        EC2 WireGuard Gateway
        10.0.1.40

        WireGuard Interface
        172.168.100.1

══════════════════════════════════════

WireGuard Tunnel

172.168.100.0/24

══════════════════════════════════════

Azure

Virtual Network
10.1.0.0/16

└── Public Subnet
    10.1.1.0/24

        Azure WireGuard VM
        10.1.1.4

        WireGuard Interface
        172.168.100.2
```

---

# Addressing Design

The addressing plan separates infrastructure into three logical networks.

## AWS Network

```
10.0.0.0/16
```

Contains:

- EC2 Instances
- Future application servers
- Future databases
- AWS infrastructure

---

## Azure Network

```
10.1.0.0/16
```

Contains:

- Azure Virtual Machines
- Future Active Directory
- Future DNS
- Future File Services

---

## WireGuard Network

```
172.168.100.0/24
```

Contains:

- AWS WireGuard interface
- Azure WireGuard interface

No production workloads reside on the WireGuard subnet.

---

# Routing Relationships

AWS routes the following network through the VPN:

```
10.1.0.0/16
```

Azure routes the following network through the VPN:

```
10.0.0.0/16
```

The WireGuard tunnel itself uses:

```
172.168.100.0/24
```

---

# Addressing Strategy

The IP addressing plan was designed to:

- Eliminate overlapping address ranges.
- Simplify VPN routing.
- Support future network expansion.
- Maintain clear separation between cloud providers.
- Enable enterprise-scale growth.

---

# Benefits

This addressing design provides:

- Predictable addressing
- Simplified troubleshooting
- Secure hybrid networking
- Scalable cloud infrastructure
- Clean routing between cloud providers
- No requirement for Network Address Translation (NAT)

---

# Verification

The addressing plan was successfully validated through:

- WireGuard tunnel establishment
- Successful peer handshake
- AWS to Azure connectivity
- Azure to AWS connectivity
- Private IP communication across both cloud providers

Verification screenshots are available in:

```text
images/testing/verification/
```

---

# Skills Demonstrated

- IPv4 Addressing
- CIDR Subnetting
- AWS Networking
- Azure Networking
- Hybrid Cloud Design
- VPN Address Planning
- Enterprise Network Architecture
- Routing Design

---

# Outcome

A structured IP addressing scheme was successfully implemented for the Enterprise Hybrid Cloud Platform. Independent private networks for AWS, Azure, and the WireGuard tunnel provide a scalable foundation for secure hybrid cloud communication while enabling routed connectivity without Network Address Translation.