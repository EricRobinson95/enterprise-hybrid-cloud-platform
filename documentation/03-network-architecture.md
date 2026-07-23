# 03 - Network Architecture

# Overview

The Enterprise Hybrid Cloud Platform implements a production-style hybrid cloud network connecting Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware environment through an encrypted WireGuard hub-and-spoke VPN.

AWS functions as the central routing hub while Azure and the on-premises environment operate as spoke sites. All inter-site communication traverses the AWS WireGuard gateway, providing centralized routing, simplified management, and secure encrypted communication between enterprise locations.

The architecture simulates a real-world enterprise network supporting centralized identity management, hybrid cloud networking, Windows administration, Linux routing, secure remote administration, and future production application hosting.

---

# Network Summary

| Environment | CIDR |
|-------------|-------------|
| AWS Production | 10.0.0.0/16 |
| Azure Enterprise | 10.1.0.0/16 |
| On-Premises Enterprise | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# Enterprise Network Topology

```
                          Internet
                              │
                       Cloudflare DNS
                              │
                              ▼

                 AWS Production Environment
                   WireGuard VPN Hub
                     10.0.0.0/16
                    WG: 172.16.100.1
                              │
          ┌───────────────────┴───────────────────┐
          │                                       │
          ▼                                       ▼

 Azure Enterprise                     On-Premises Enterprise
     10.1.0.0/16                           10.2.0.0/16

 WireGuard Gateway                     Ubuntu WireGuard Gateway
 Windows Server 2025                   VMware Workstation
 Active Directory                      Windows 11 Enterprise
 DNS                                  Kali Linux
```

All inter-site communication is routed through the AWS WireGuard hub.

---

# AWS Production Network

AWS serves as the production networking hub for the hybrid cloud.

## VPC

```
10.0.0.0/16
```

---

## Public Subnet

```
10.0.1.0/24
```

Current Resources

- Internet Gateway
- Ubuntu WireGuard Gateway
- SSH Administration

Gateway Interfaces

```
eth0    10.0.1.40
wg0     172.16.100.1
```

Responsibilities

- WireGuard Hub
- VPN Termination
- Linux IP Forwarding
- Packet Routing
- Static Routing
- SSH Administration

---

## Private Application Subnet

```
10.0.2.0/24
```

Future Resources

- Amazon ECS
- FastAPI Backend
- React Services

---

## Private Database Subnet

```
10.0.3.0/24
```

Future Resources

- PostgreSQL
- Amazon RDS

---

# Azure Enterprise Network

Azure provides centralized enterprise services.

## Virtual Network

```
10.1.0.0/16
```

---

## Gateway Subnet

```
10.1.1.0/24
```

Gateway Interfaces

```
eth0    10.1.1.4
wg0     172.16.100.2
```

Responsibilities

- WireGuard Gateway
- Secure Routing
- Linux IP Forwarding

---

## Identity Services Subnet

```
10.1.2.0/24
```

Current Resources

- Windows Server 2025
- Active Directory Domain Services
- DNS
- Organizational Units
- Enterprise Users
- Security Groups

---

## Enterprise Client Subnet

```
10.1.3.0/24
```

Current Resources

- Windows 11 Enterprise
- Domain Joined Workstation

---

## Infrastructure Services Subnet

```
10.1.4.0/24
```

Future Resources

- Windows File Server
- Internal Application Server

---

# On-Premises Enterprise Network

The on-premises environment simulates an enterprise office and security operations center.

## VMware Network

```
10.2.0.0/16
```

---

## Gateway Subnet

```
10.2.1.0/24
```

Gateway Interfaces

```
eth0    10.2.1.12
wg0     172.16.100.3
```

Responsibilities

- WireGuard Gateway
- Enterprise Routing
- Secure SSH Administration

---

## Security Operations Subnet

```
10.2.2.0/24
```

Current Resources

- Kali Linux

Purpose

- Vulnerability Assessment
- Network Analysis
- Burp Suite
- OWASP ZAP

---

## Enterprise Workstation Subnet

```
10.2.3.0/24
```

Current Resources

- Windows 11 Enterprise
- Domain Joined Workstation

Purpose

- Enterprise Administration
- Application Validation
- Cross-Site Authentication

---

# WireGuard VPN

Tunnel Network

```
172.16.100.0/24
```

| Device | Tunnel Address |
|----------|----------------|
| AWS Gateway | 172.16.100.1 |
| Azure Gateway | 172.16.100.2 |
| On-Premises Gateway | 172.16.100.3 |

---

# Enterprise Routing

The routing infrastructure provides Layer 3 connectivity between every enterprise network.

## AWS

Routing Components

- VPC Route Tables
- Linux Routing
- WireGuard

Advertised Networks

```
10.1.0.0/16
10.2.0.0/16
```

---

## Azure

Routing Components

- User Defined Routes (UDRs)
- Linux Routing
- WireGuard

Advertised Networks

```
10.0.0.0/16
10.2.0.0/16
```

---

## On-Premises

Routing Components

- Linux Static Routes
- Windows Persistent Routes
- WireGuard

Advertised Networks

```
10.0.0.0/16
10.1.0.0/16
```

---

# Enterprise Services

## AWS

Current

- WireGuard VPN Hub
- Linux Router
- Internet Gateway

Future

- React
- FastAPI
- PostgreSQL
- Application Load Balancer
- CloudWatch

---

## Azure

Current

- Active Directory
- DNS
- Windows Server 2025
- Enterprise Users
- Organizational Units
- Security Groups

Future

- File Server
- Group Policy
- Internal Applications

---

## On-Premises

Current

- VMware
- Ubuntu Gateway
- Windows 11 Enterprise
- Kali Linux

Future

- Security Automation
- Network Monitoring

---

# Network Validation

The networking infrastructure has been validated through end-to-end testing.

## WireGuard

Validated

- AWS ↔ Azure
- AWS ↔ On-Premises

Verification

```
wg
```

---

## Routing

Validated

- AWS Route Tables
- Azure User Defined Routes
- Linux Static Routes
- Windows Persistent Routes

Verification

```
ip route
route print
```

---

## Connectivity

Validated

- AWS → Azure
- Azure → AWS
- AWS → On-Premises
- On-Premises → AWS
- Azure → On-Premises
- On-Premises → Azure

Verification

```
ping
```

---

## Path Validation

Validated

- Cross-site routing

Verification

```
tracert
```

---

## DNS Validation

Validated

- Cross-site DNS resolution

Verification

```
nslookup
```

---

## Active Directory

Validated

- Domain Authentication
- Enterprise Users
- Domain Joined Windows 11 Workstations

Verification

```
whoami
Get-ADUser
Get-ADComputer
```

---

## Secure Administration

Validated

- SSH administration
- Windows Remote Desktop
- Cross-site management

---

# Architecture Highlights

- Hybrid Cloud Architecture
- Enterprise Hub-and-Spoke VPN
- Multi-Cloud Networking
- WireGuard Site-to-Site VPN
- AWS Route Tables
- Azure User Defined Routes
- Linux Routing
- Windows Persistent Routes
- Active Directory Domain Services
- Enterprise DNS
- Domain Joined Windows 11 Workstations
- Cross-Site Authentication
- Cross-Site DNS Resolution
- Secure Remote Administration
- VMware Enterprise Lab

---

# Current Status

## Completed

### Networking

- AWS Production Network
- Azure Enterprise Network
- On-Premises Enterprise Network
- WireGuard Hub-and-Spoke VPN
- Enterprise Routing
- Linux IP Forwarding
- AWS Route Tables
- Azure User Defined Routes
- Windows Persistent Routes
- Cross-Site Connectivity

### Identity

- Windows Server 2025
- Active Directory Domain Services
- DNS
- Organizational Units
- Enterprise Users
- Security Groups
- Domain Joined Windows 11 Workstations

### Validation

- WireGuard Tunnel Verification
- Routing Verification
- ICMP Connectivity
- Route Tracing
- DNS Resolution
- Active Directory Authentication
- Secure SSH Administration

---

# Future Expansion

- Windows File Server
- Group Policy
- Internal Application Server
- React Frontend
- FastAPI Backend
- PostgreSQL
- GitHub Actions Deployment
- Cloud Monitoring
- Security Automation

---

# Summary

The Enterprise Hybrid Cloud Platform implements a secure, production-style hybrid cloud architecture that integrates AWS, Azure, and an on-premises enterprise environment through an encrypted WireGuard hub-and-spoke VPN.

The completed network provides enterprise routing, centralized Active Directory, DNS, domain-joined Windows workstations, cross-site authentication, secure remote administration, and validated communication between every environment. The architecture establishes a scalable networking foundation for future application hosting, DevOps automation, enterprise services, and security operations.