# IP Addressing Plan

## Overview

This document defines the enterprise IP addressing scheme used throughout the hybrid cloud platform. The environment consists of three independent private networks connected through an encrypted WireGuard hub-and-spoke VPN.

| Environment | Network |
|------------|------------|
| AWS Cloud | 10.0.0.0/16 |
| Azure Cloud | 10.1.0.0/16 |
| On-Premises VMware | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# Global Network Allocation

| Network | Purpose |
|----------|---------|
| 10.0.0.0/16 | AWS Cloud |
| 10.1.0.0/16 | Azure Cloud |
| 10.2.0.0/16 | On-Premises Lab |
| 172.16.100.0/24 | WireGuard Tunnel Network |

---

# AWS Cloud

## VPC

```
10.0.0.0/16
```

| Subnet | CIDR | Purpose |
|---------|------|----------|
| Public | 10.0.1.0/24 | Internet-facing resources |
| Application | 10.0.2.0/24 | ECS Cluster |
| Database | 10.0.3.0/24 | Amazon RDS PostgreSQL |

---

## AWS Hosts

| Device | Interface | Address |
|---------|-----------|---------|
| Internet Gateway | N/A | AWS Managed |
| Application Load Balancer | Public | Dynamic |
| WireGuard Gateway | eth0 | 10.0.1.40 |
| WireGuard Gateway | wg0 | 172.16.100.1 |

---

# Azure Cloud

## Virtual Network

```
10.1.0.0/16
```

| Subnet | CIDR | Purpose |
|---------|------|----------|
| Public | 10.1.1.0/24 | VPN Gateway |
| Identity Services | 10.1.2.0/24 | Active Directory |
| Client | 10.1.3.0/24 | Windows 11 Workstations |
| Infrastructure Services | 10.1.4.0/24 | File Server & Internal Applications |

---

## Azure Hosts

| Device | Interface | Address |
|---------|-----------|---------|
| Ubuntu WireGuard Gateway | eth0 | 10.1.1.4 |
| Ubuntu WireGuard Gateway | wg0 | 172.16.100.2 |
| Windows Server 2025 | Ethernet | 10.1.2.x *(Static)* |
| Windows 11 Client *(Planned)* | Ethernet | 10.1.3.x |
| File Server *(Planned)* | Ethernet | 10.1.4.x |
| Internal Application Server *(Planned)* | Ethernet | 10.1.4.x |

---

# On-Premises Network

## VMware VMnet8 Network

```
10.2.0.0/16
```

| Subnet | CIDR | Purpose |
|---------|------|----------|
| Public | 10.2.1.0/24 | WireGuard Gateway |
| Security Operations | 10.2.2.0/24 | Kali Linux |
| Test Lab | 10.2.3.0/24 | Windows Test Machines |

---

## On-Premises Hosts

| Device | Interface | Address |
|---------|-----------|---------|
| Ubuntu WireGuard Gateway | ens33 | 10.2.1.12 |
| Ubuntu WireGuard Gateway | wg0 | 172.16.100.3 |
| Kali Linux | ens33 | 10.2.2.x |
| Windows 11 Test Workstation *(Planned)* | Ethernet | 10.2.3.x |

---

# WireGuard Tunnel Network

```
172.16.100.0/24
```

| Gateway | Tunnel IP |
|----------|-----------|
| AWS Hub | 172.16.100.1 |
| Azure Spoke | 172.16.100.2 |
| On-Premises Spoke | 172.16.100.3 |

---

# Routing Summary

## AWS Hub

Routes

```
10.1.0.0/16
10.2.0.0/16
```

---

## Azure Spoke

Routes

```
10.0.0.0/16
10.2.0.0/16
```

---

## On-Premises Spoke

Routes

```
10.0.0.0/16
10.1.0.0/16
```

---

# Default Gateways

| Environment | Default Gateway |
|-------------|----------------|
| AWS | 10.0.1.1 |
| Azure | 10.1.1.1 |
| On-Premises | 10.2.1.1 |

---

# DNS Configuration

| Environment | DNS Server |
|-------------|------------|
| AWS | AWS VPC Resolver |
| Azure | Windows Server 2025 (Active Directory DNS) |
| On-Premises | Windows Server 2025 DNS (via WireGuard) *(planned)* |

---

# VPN Address Advertisement

## AWS Hub

Advertises

```
10.1.0.0/16
10.2.0.0/16
```

---

## Azure Spoke

Advertises

```
10.0.0.0/16
10.2.0.0/16
172.16.100.1/32
172.16.100.3/32
```

---

## On-Premises Spoke

Advertises

```
10.0.0.0/16
10.1.0.0/16
172.16.100.1/32
172.16.100.2/32
```

---

# Addressing Standards

- Every cloud environment uses a unique /16 private network.
- Every subnet uses a /24 CIDR block.
- WireGuard gateways use static IP addresses.
- Windows infrastructure servers use static IP addresses.
- Client workstations use reserved static addresses or DHCP reservations.
- VPN tunnel addresses are assigned from the dedicated 172.16.100.0/24 network.

---

# Current Address Utilization

| Environment | Status |
|-------------|--------|
| AWS | Operational |
| Azure | Operational |
| On-Premises | Operational |
| WireGuard Tunnel | Operational |
| Azure Active Directory | Operational |
| Windows Server 2025 | Operational |
| Azure Windows 11 Client | Planned |
| On-Premises Windows 11 Client | Planned |
| File Server | Planned |
| Internal Application Server | Planned |