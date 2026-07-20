# 04 - IP Addressing

# Overview

This document defines the IP addressing scheme used throughout the Enterprise Hybrid Cloud Platform.

The environment uses private RFC 1918 IPv4 address space and is segmented across Amazon Web Services (AWS), Microsoft Azure, and an on-premises security lab. Each environment has its own dedicated network to simplify routing, improve security, and support future scalability.

---

# IP Addressing Objectives

- Provide unique address space for each environment
- Eliminate overlapping networks
- Simplify static routing
- Support WireGuard site-to-site VPN connectivity
- Separate production, corporate, and security environments
- Allow future expansion

---

# Enterprise Address Space

| Environment | Network | Purpose |
|------------|----------|----------|
| AWS | 10.0.0.0/16 | Production Environment |
| Azure | 10.1.0.0/16 | Corporate IT |
| On-Premises | 10.2.0.0/16 | Security Operations |
| WireGuard Tunnel | 172.16.100.0/24 | VPN Transit Network |

---

# AWS Production Environment

Network

```
10.0.0.0/16
```

## Subnets

| Subnet | Network | Purpose |
|---------|----------|----------|
| Public Subnet | 10.0.1.0/24 | Load Balancer, WireGuard Gateway |
| Application Subnet | 10.0.2.0/24 | React & FastAPI |
| Database Subnet | 10.0.3.0/24 | PostgreSQL |

---

## Planned Resources

| Resource | Private IP |
|-----------|------------|
| WireGuard Hub | 10.0.1.40 |
| Application Load Balancer | Dynamic |
| React Application | Dynamic |
| FastAPI Server | Dynamic |
| PostgreSQL Database | Dynamic |

---

# Azure Corporate Environment

Network

```
10.1.0.0/16
```

## Subnets

| Subnet | Network | Purpose |
|---------|----------|----------|
| Public Subnet | 10.1.1.0/24 | WireGuard Gateway |
| Identity Services Subnet | 10.1.2.0/24 | Active Directory & DNS |
| Developer Subnet | 10.1.3.0/24 | Windows 11 Developer Workstation |
| Infrastructure Services Subnet | 10.1.4.0/24 | File Server & Internal Applications |

---

## Planned Resources

| Resource | Private IP |
|-----------|------------|
| Ubuntu WireGuard Gateway | 10.1.1.4 |
| Windows Server 2025 | 10.1.2.10 |
| Windows File Server | 10.1.4.10 |
| Internal Application Server | 10.1.4.20 |
| Windows 11 Developer Workstation | 10.1.3.10 |

---

# On-Premises Security Lab

Network

```
10.2.0.0/16
```

## Subnets

| Subnet | Network | Purpose |
|---------|----------|----------|
| Public Subnet | 10.2.1.0/24 | WireGuard Gateway |
| Security Operations Subnet | 10.2.2.0/24 | Kali Linux |
| Security Testing Subnet | 10.2.3.0/24 | Security Test Workstation |

---

## Planned Resources

| Resource | Private IP |
|-----------|------------|
| Ubuntu WireGuard Gateway | 10.2.1.12 |
| Kali Linux | 10.2.2.10 |
| Security Test Workstation | 10.2.3.10 |

---

# WireGuard Tunnel Network

Network

```
172.16.100.0/24
```

## Tunnel Addresses

| Device | Tunnel IP |
|----------|-----------|
| AWS WireGuard Hub | 172.16.100.1 |
| Azure WireGuard Gateway | 172.16.100.2 |
| On-Premises WireGuard Gateway | 172.16.100.3 |

---

# Network Summary

```
AWS Production
10.0.0.0/16

├── 10.0.1.0/24 Public
├── 10.0.2.0/24 Application
└── 10.0.3.0/24 Database


Azure Corporate
10.1.0.0/16

├── 10.1.1.0/24 Public
├── 10.1.2.0/24 Identity Services
├── 10.1.3.0/24 Developer
└── 10.1.4.0/24 Infrastructure


On-Premises Security Lab
10.2.0.0/16

├── 10.2.1.0/24 Public
├── 10.2.2.0/24 Security Operations
└── 10.2.3.0/24 Security Testing
```

---

# VPN Transit Network

```
172.16.100.0/24

AWS Hub
172.16.100.1

Azure Gateway
172.16.100.2

On-Premises Gateway
172.16.100.3
```

---

# Addressing Strategy

The addressing plan follows a simple hierarchical structure.

```
10.0.x.x

AWS Production
```

```
10.1.x.x

Azure Corporate
```

```
10.2.x.x

On-Premises Security
```

This structure makes the environment easy to understand and simplifies troubleshooting.

---

# Routing Summary

| Source | Destination | Route |
|----------|-------------|-------|
| AWS | Azure | WireGuard |
| AWS | On-Premises | WireGuard |
| Azure | AWS | WireGuard |
| On-Premises | AWS | WireGuard |

Current implementation uses AWS as the central routing hub.

---

# Reserved Address Space

The following address ranges remain available for future expansion.

| Network | Purpose |
|----------|----------|
| 10.0.4.0/24 - 10.0.255.0/24 | Additional AWS Services |
| 10.1.5.0/24 - 10.1.255.0/24 | Additional Azure Services |
| 10.2.4.0/24 - 10.2.255.0/24 | Additional On-Premises Networks |

---

# Future Expansion

Future infrastructure may include:

## AWS

- Bastion Host
- Container Services
- Kubernetes Cluster
- Monitoring Servers

---

## Azure

- Microsoft Entra ID Integration
- Azure Bastion
- Backup Services
- Update Management

---

## On-Premises

- SIEM
- Logging Server
- Vulnerability Scanner
- Additional Security Workstations

---

# Summary

The Enterprise Hybrid Cloud Platform uses a structured private IPv4 addressing scheme that separates production, corporate IT, and security operations into dedicated networks.

The addressing plan supports secure WireGuard site-to-site VPN connectivity, simplifies routing, avoids overlapping address space, and provides sufficient capacity for future growth while maintaining a clear and scalable enterprise network design.