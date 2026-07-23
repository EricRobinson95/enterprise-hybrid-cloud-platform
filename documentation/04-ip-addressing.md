# 04 - IP Addressing Plan

# Overview

This document defines the enterprise IP addressing scheme used throughout the Enterprise Hybrid Cloud Platform.

The infrastructure consists of three private enterprise networks connected through an encrypted WireGuard hub-and-spoke VPN. Each environment uses a dedicated RFC1918 address space to simplify routing, improve scalability, and prevent address overlap.

---

# Enterprise Network Allocation

| Environment | CIDR |
|-------------|-------------|
| AWS Production | 10.0.0.0/16 |
| Azure Enterprise | 10.1.0.0/16 |
| On-Premises Enterprise | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# Global Address Space

| Network | Purpose |
|----------|----------|
| 10.0.0.0/16 | AWS Production Environment |
| 10.1.0.0/16 | Azure Enterprise Environment |
| 10.2.0.0/16 | On-Premises Enterprise Environment |
| 172.16.100.0/24 | WireGuard VPN Tunnel Network |

---

# AWS Production Network

## VPC

```
10.0.0.0/16
```

## Subnets

| Subnet | CIDR | Purpose |
|---------|------|----------|
| Public | 10.0.1.0/24 | Internet-facing resources |
| Application | 10.0.2.0/24 | Future application services |
| Database | 10.0.3.0/24 | Future PostgreSQL database |

---

## Current Hosts

| Device | Interface | IP Address |
|---------|-----------|------------|
| Internet Gateway | AWS Managed | N/A |
| Ubuntu WireGuard Gateway | eth0 | 10.0.1.40 |
| Ubuntu WireGuard Gateway | wg0 | 172.16.100.1 |

---

# Azure Enterprise Network

## Virtual Network

```
10.1.0.0/16
```

## Subnets

| Subnet | CIDR | Purpose |
|---------|------|----------|
| Gateway | 10.1.1.0/24 | WireGuard Gateway |
| Identity Services | 10.1.2.0/24 | Active Directory & DNS |
| Enterprise Clients | 10.1.3.0/24 | Windows 11 Workstations |
| Infrastructure Services | 10.1.4.0/24 | Future Enterprise Services |

---

## Current Hosts

| Device | Interface | IP Address |
|---------|-----------|------------|
| Ubuntu WireGuard Gateway | eth0 | 10.1.1.4 |
| Ubuntu WireGuard Gateway | wg0 | 172.16.100.2 |
| Windows Server 2025 | Ethernet | 10.1.2.4 |
| Windows 11 Enterprise | Ethernet | 10.1.3.x |

---

## Future Hosts

| Device | Planned Address |
|---------|----------------|
| Windows File Server | 10.1.4.x |
| Internal Application Server | 10.1.4.x |

---

# On-Premises Enterprise Network

## VMware Network

```
10.2.0.0/16
```

## Subnets

| Subnet | CIDR | Purpose |
|---------|------|----------|
| Gateway | 10.2.1.0/24 | WireGuard Gateway |
| Security Operations | 10.2.2.0/24 | Kali Linux |
| Enterprise Clients | 10.2.3.0/24 | Windows 11 Workstations |

---

## Current Hosts

| Device | Interface | IP Address |
|---------|-----------|------------|
| Ubuntu WireGuard Gateway | eth0 | 10.2.1.12 |
| Ubuntu WireGuard Gateway | wg0 | 172.16.100.3 |
| Kali Linux | Ethernet | 10.2.2.x |
| Windows 11 Enterprise | Ethernet | 10.2.3.x |

---

# WireGuard Tunnel Network

```
172.16.100.0/24
```

| Gateway | Tunnel Address |
|----------|----------------|
| AWS Hub | 172.16.100.1 |
| Azure Gateway | 172.16.100.2 |
| On-Premises Gateway | 172.16.100.3 |

---

# Enterprise Routing

## AWS

Routing Components

- AWS Route Tables
- Linux Static Routes

Remote Networks

```
10.1.0.0/16
10.2.0.0/16
```

---

## Azure

Routing Components

- User Defined Routes (UDRs)
- Linux Routing

Remote Networks

```
10.0.0.0/16
10.2.0.0/16
```

---

## On-Premises

Routing Components

- Linux Static Routes
- Windows Persistent Routes

Remote Networks

```
10.0.0.0/16
10.1.0.0/16
```

---

# Default Gateways

| Environment | Gateway |
|-------------|---------|
| AWS | 10.0.1.1 |
| Azure | 10.1.1.1 |
| On-Premises | 10.2.1.1 |

---

# DNS Configuration

| Environment | Primary DNS |
|-------------|-------------|
| AWS | Amazon VPC Resolver |
| Azure | Windows Server 2025 (10.1.2.4) |
| On-Premises | Windows Server 2025 DNS (10.1.2.4) |

Active Directory DNS provides enterprise name resolution across the hybrid cloud through the WireGuard VPN.

---

# WireGuard Address Advertisement

## AWS Hub

Advertises

```
10.1.0.0/16
10.2.0.0/16
```

---

## Azure Gateway

Advertises

```
10.0.0.0/16
10.2.0.0/16
```

---

## On-Premises Gateway

Advertises

```
10.0.0.0/16
10.1.0.0/16
```

---

# Addressing Standards

The environment follows these addressing standards:

- Each environment uses a dedicated /16 private network.
- Each subnet uses a /24 allocation.
- Infrastructure servers use static IP addresses.
- WireGuard gateways use static addresses.
- Active Directory services use static addressing.
- Windows enterprise workstations use reserved addresses or DHCP reservations.
- VPN tunnel interfaces use dedicated addresses from the 172.16.100.0/24 network.

---

# Current Address Utilization

| Infrastructure | Status |
|----------------|--------|
| AWS Production Network | Operational |
| Azure Enterprise Network | Operational |
| On-Premises Enterprise Network | Operational |
| WireGuard VPN | Operational |
| Windows Server 2025 | Operational |
| Active Directory Domain Services | Operational |
| DNS Services | Operational |
| Azure Windows 11 Enterprise | Operational |
| On-Premises Windows 11 Enterprise | Operational |

---

# Reserved Address Space

The following address ranges are reserved for future expansion:

| Network | Planned Services |
|----------|------------------|
| 10.0.2.0/24 | Application Services |
| 10.0.3.0/24 | Database Services |
| 10.1.4.0/24 | Windows File Server |
| 10.1.4.0/24 | Internal Application Server |

---

# Summary

The Enterprise Hybrid Cloud Platform uses a structured RFC1918 addressing scheme to provide scalable, non-overlapping IP space across AWS, Azure, and the on-premises enterprise environment.

Static addressing is used for infrastructure services, WireGuard gateways, and Active Directory components, while enterprise routing, DNS resolution, and cross-site communication are provided through the WireGuard hub-and-spoke VPN. This addressing plan establishes a scalable foundation for future enterprise services, application hosting, and infrastructure expansion.