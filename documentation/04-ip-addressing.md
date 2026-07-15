# 04 - IP Addressing

# Enterprise Hybrid Cloud Platform - IP Addressing Plan

## Overview

This document defines the IP addressing scheme used throughout the Enterprise Hybrid Cloud Platform.

The addressing plan has been designed to:

- Support a scalable hybrid cloud architecture
- Separate workloads using network segmentation
- Prevent overlapping IP address ranges
- Simplify routing between environments
- Support secure site-to-site VPN connectivity
- Allow future network expansion

Private IPv4 addressing (RFC 1918) is used throughout the platform.

---

# Addressing Strategy

Each major environment is assigned its own dedicated private network.

| Environment | CIDR Block |
|-------------|------------|
| AWS VPC | 10.0.0.0/16 |
| Azure VNet | 10.1.0.0/16 |
| On-Premises Network | 10.2.0.0/16 |
| WireGuard VPN Overlay | 10.255.0.0/24 |

This addressing strategy provides clear separation between environments while allowing secure communication through the site-to-site VPN.

---

# AWS Network

## VPC

| Property | Value |
|----------|-------|
| Network | AWS VPC |
| CIDR Block | 10.0.0.0/16 |

The AWS VPC hosts the public-facing application environment.

### AWS Subnets

| Subnet | CIDR | Purpose |
|---------|------|---------|
| Public Subnet | 10.0.1.0/24 | Internet-facing resources |
| Private Application Subnet | 10.0.2.0/24 | Amazon ECS |
| Private Database Subnet | 10.0.3.0/24 | Amazon RDS |

Future subnets may be added as the platform expands.

---

# Azure Network

## Virtual Network (VNet)

| Property | Value |
|----------|-------|
| Network | Azure Virtual Network |
| CIDR Block | 10.1.0.0/16 |

The Azure Virtual Network hosts enterprise infrastructure services.

### Azure Subnets

| Subnet | CIDR | Purpose |
|---------|------|---------|
| Identity Services | 10.1.1.0/24 | Active Directory and DNS |
| Management | 10.1.2.0/24 | Administrative resources |
| Infrastructure Services | 10.1.3.0/24 | File Server, Monitoring, Backup |

A dedicated **GatewaySubnet** will be created during implementation to support the Azure VPN Gateway if required.

---

# On-Premises Network

| Property | Value |
|----------|-------|
| Network | On-Premises |
| CIDR Block | 10.2.0.0/16 |

The on-premises environment provides local administration, virtualization, and future enterprise networking infrastructure.

Initial resources include:

- Windows 11 Workstation
- VMware Workstation Pro
- Ubuntu Server VPN Gateway
- ClockworkPi uConsole

Future infrastructure may include Cisco routing and switching equipment.

---

# WireGuard VPN Overlay Network

WireGuard uses a dedicated VPN overlay network for encrypted tunnel interfaces.

| Property | Value |
|----------|-------|
| Network | WireGuard Overlay |
| CIDR Block | 10.255.0.0/24 |

### Planned Tunnel Interface Addresses

| Device | VPN Address |
|---------|-------------|
| AWS WireGuard Gateway | 10.255.0.1 |
| Azure WireGuard Gateway | 10.255.0.2 |
| On-Premises WireGuard Gateway | 10.255.0.3 |

These addresses are used only by the VPN tunnel interfaces and are not assigned to application or infrastructure workloads.

---

# Route Advertisement

Each VPN gateway advertises the private network it owns.

| Gateway | Advertised Network |
|----------|--------------------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |

These advertised routes allow secure communication between cloud environments without exposing internal services to the public Internet.

---

# Address Allocation Principles

The addressing plan follows several design principles.

- One /16 network per environment
- One /24 subnet per workload
- No overlapping IP ranges
- Private addressing only
- Dedicated VPN overlay network
- Reserved address space for future growth

This structure simplifies routing, network management, and future expansion.

---

# Future Expansion

The addressing plan provides sufficient capacity for additional services.

Future additions may include:

- Additional AWS application subnets
- Additional Azure infrastructure subnets
- Development environment
- Testing environment
- Dedicated management networks
- Physical Cisco infrastructure
- Additional branch office networks

---

# Summary

The Enterprise Hybrid Cloud Platform uses a structured private IPv4 addressing scheme that separates AWS, Azure, the on-premises environment, and the WireGuard VPN overlay into dedicated network ranges. This design supports secure routing, scalable growth, and simplified administration while providing a solid foundation for the platform's hybrid cloud architecture.