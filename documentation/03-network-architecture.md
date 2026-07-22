# Network Architecture

## Overview

This project implements a production-style hybrid cloud architecture connecting Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware environment through an encrypted WireGuard hub-and-spoke VPN topology.

AWS functions as the central networking hub while Azure and the on-premises environment operate as spoke sites. All inter-site traffic traverses the AWS WireGuard gateway, eliminating the need for direct tunnels between Azure and the on-premises network.

The environment simulates an enterprise infrastructure supporting identity services, application hosting, hybrid networking, secure remote administration, and future production workloads.

---

# Architecture Summary

| Environment | Network |
|------------|------------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# Hub-and-Spoke VPN Design

```
                     Internet
                         │
                    Cloudflare DNS
                         │
                AWS WireGuard Hub
                 172.16.100.1
                         │
          ┌──────────────┴──────────────┐
          │                             │
          │                             │
 Azure WireGuard                 On-Premises WireGuard
 172.16.100.2                     172.16.100.3
      │                               │
 Azure Network                  VMware Network
10.1.0.0/16                     10.2.0.0/16
```

AWS advertises both remote networks and performs routing between them.

Azure and the on-premises environment never communicate directly across the Internet.

---

# AWS Cloud Architecture

## VPC

```
10.0.0.0/16
```

### Public Subnet

```
10.0.1.0/24
```

Resources

- Internet Gateway
- Application Load Balancer
- Ubuntu EC2 WireGuard VPN Gateway

Ubuntu VPN Gateway

```
eth0 10.0.1.40
wg0 172.16.100.1
```

Responsibilities

- WireGuard Hub
- Linux IP Forwarding
- Packet Routing
- VPN Termination
- Route Advertisement
- NAT
- SSH Administration

---

### Private Application Subnet

```
10.0.2.0/24
```

Resources

- Amazon ECS Cluster
- Future containerized applications

---

### Private Database Subnet

```
10.0.3.0/24
```

Resources

- Amazon RDS PostgreSQL

---

# Azure Cloud Architecture

## Virtual Network

```
10.1.0.0/16
```

---

### Public Subnet

```
10.1.1.0/24
```

Resources

Ubuntu WireGuard Gateway

```
eth0 10.1.1.4
wg0 172.16.100.2
```

Responsibilities

- Azure VPN Gateway
- Secure routing to AWS
- Secure routing to On-Premises

---

### Identity Services Subnet

```
10.1.2.0/24
```

Resources

- Windows Server 2025
- Active Directory Domain Services
- DNS
- Organizational Units
- User Accounts
- Security Groups

---

### Client Subnet

```
10.1.3.0/24
```

Planned Resources

- Windows 11 Enterprise Client
- Domain Joined Workstation

---

### Infrastructure Services Subnet

```
10.1.4.0/24
```

Planned Resources

- Windows File Server
- Internal Application Server

---

# On-Premises Architecture

## VMware VMnet8 Network

```
10.2.0.0/16
```

---

### Public Subnet

```
10.2.1.0/24
```

Resources

Ubuntu WireGuard Gateway

```
eth0 10.2.1.12
wg0 172.16.100.3
```

Responsibilities

- VPN Gateway
- Linux Router
- Secure SSH Administration

---

### Security Operations Subnet

```
10.2.2.0/24
```

Resources

- Kali Linux

Purpose

- Penetration Testing
- Security Assessments
- Burp Suite Testing
- SSH Administration

---

### Test Lab Subnet

```
10.2.3.0/24
```

Resources

- Windows Test Workstation

Purpose

- Application Testing
- Production Validation

---

# WireGuard Tunnel Network

```
172.16.100.0/24
```

| Device | Tunnel Address |
|----------|----------------|
| AWS Hub | 172.16.100.1 |
| Azure Gateway | 172.16.100.2 |
| On-Premises Gateway | 172.16.100.3 |

---

# Advertised Networks

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
172.16.100.1/32
172.16.100.3/32
```

---

## On-Premises Gateway

Advertises

```
10.0.0.0/16
10.1.0.0/16
172.16.100.1/32
172.16.100.2/32
```

---

# Routing

AWS

```
10.1.0.0/16 → Azure
10.2.0.0/16 → On-Premises
```

Azure

```
10.0.0.0/16 → AWS
10.2.0.0/16 → AWS
```

On-Premises

```
10.0.0.0/16 → AWS
10.1.0.0/16 → AWS
```

---

# Network Services

AWS

- Internet Gateway
- Application Load Balancer
- WireGuard VPN
- Amazon ECS
- Amazon RDS

Azure

- Active Directory
- DNS
- Windows Server 2025
- Azure Route Tables
- Network Security Groups

On-Premises

- VMware Workstation
- Kali Linux
- Ubuntu VPN Gateway
- Test Workstation

---

# Connectivity Validation

The following tests have been successfully completed.

## VPN Handshakes

✅ AWS ↔ Azure

✅ AWS ↔ On-Premises

---

## Routing

Verified

```
ip route
```

on

- AWS
- Azure
- On-Premises

---

## WireGuard Status

Verified

```
wg
```

on

- AWS
- Azure
- On-Premises

---

## Tunnel Configuration

Verified

```
wg0.conf
```

on all VPN gateways.

---

## ICMP Testing

Successful

AWS → Azure

Azure → AWS

AWS → On-Premises

On-Premises → AWS

Azure → On-Premises

On-Premises → Azure

---

## Packet Capture

Traffic verified using

```
tcpdump
```

showing encrypted ICMP packets traversing the WireGuard tunnel.

---

# Current Project Status

Completed

- AWS Network
- Azure Network
- On-Premises Network
- Hub-and-Spoke VPN
- WireGuard Routing
- Linux Routing
- AWS EC2
- Azure VM
- VMware Gateway
- Active Directory Installation
- DNS Installation
- User Creation
- Organizational Units
- Security Groups
- Secure SSH Access
- Cross-site Connectivity Validation

Remaining

- Azure Windows 11 Client
- On-Premises Windows 11 Client
- Domain Join Workstations
- Group Policy Validation
- File Server Deployment
- Internal Application Server Deployment

---

# Architecture Highlights

- Enterprise Hub-and-Spoke VPN
- Multi-Cloud Networking
- Hybrid Cloud Architecture
- WireGuard Site-to-Site VPN
- Active Directory Identity Services
- Cross-Cloud Routing
- Linux VPN Gateways
- AWS + Azure + VMware Integration
- Secure Enterprise Segmentation
- Production-Style Network Design