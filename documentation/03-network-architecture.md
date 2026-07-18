# 03 - Network Architecture

# Overview

This document describes the logical network architecture of the Enterprise Hybrid Cloud Platform. The environment connects Amazon Web Services (AWS) and Microsoft Azure using a secure WireGuard site-to-site VPN, creating a routed hybrid cloud infrastructure.

The architecture provides encrypted communication between cloud providers while maintaining independent private networks and secure administrative access.

---

# Architecture Objectives

- Design a secure hybrid cloud network.
- Connect AWS and Azure using a WireGuard site-to-site VPN.
- Maintain separate private networks.
- Encrypt all inter-cloud traffic.
- Provide a scalable foundation for enterprise services.
- Follow cloud networking and security best practices.

---

# High-Level Architecture

```
                               Internet
                                   │
          ┌────────────────────────┴────────────────────────┐
          │                                                 │
   AWS Elastic IP                                   Azure Static Public IP
          │                                                 │
┌─────────────────────┐                         ┌─────────────────────┐
│ AWS WireGuard VM    │=========================│ Azure WireGuard VM  │
│ Ubuntu 24.04 LTS    │   WireGuard VPN Tunnel  │ Ubuntu 24.04 LTS    │
│ WG:172.168.100.1    │=========================│ WG:172.168.100.2    │
└─────────────────────┘                         └─────────────────────┘
          │                                                 │
          │                                                 │
   AWS VPC 10.0.0.0/16                           Azure VNet 10.1.0.0/16
          │                                                 │
   Private Resources                               Private Resources
```

---

# Cloud Components

## Amazon Web Services

The AWS environment includes:

- Virtual Private Cloud (VPC)
- Public Subnet
- Internet Gateway
- Route Table
- Security Group
- Elastic IP
- Ubuntu Server 24.04 LTS EC2 Instance
- WireGuard VPN Gateway

---

## Microsoft Azure

The Azure environment includes:

- Virtual Network (VNet)
- Public Subnet
- Network Security Group
- Static Public IP
- Network Interface
- Ubuntu Server 24.04 LTS Virtual Machine
- WireGuard VPN Gateway

---

# Network Addressing

## AWS

| Resource | Address |
|----------|---------|
| VPC | 10.0.0.0/16 |
| Public Subnet | 10.0.1.0/24 |
| WireGuard Tunnel | 172.168.100.1/24 |

---

## Azure

| Resource | Address |
|----------|---------|
| VNet | 10.1.0.0/16 |
| Public Subnet | 10.1.1.0/24 |
| WireGuard Tunnel | 172.168.100.2/24 |

---

## VPN Tunnel

| Network | Address |
|----------|---------|
| WireGuard Tunnel | 172.168.100.0/24 |

---

# Communication Flow

## Administrative Access

Administrators connect securely to both VPN gateways using SSH key authentication.

```
Administrator
      │
      ▼
SSH
      │
      ▼
AWS EC2 / Azure VM
```

---

## VPN Traffic

Traffic destined for the remote cloud network is automatically routed through the WireGuard interface.

AWS

```
10.0.0.0/16
        │
        ▼
wg0
        │
Encrypted Tunnel
        │
wg0
        ▼
10.1.0.0/16
```

The same process occurs in the opposite direction for Azure.

---

# Routing Design

AWS advertises:

```
10.1.0.0/16
```

Azure advertises:

```
10.0.0.0/16
```

The VPN gateways forward only traffic destined for the remote private network.

Internet-bound traffic remains local to each cloud provider.

---

# Security Architecture

The network is protected using multiple security layers.

## AWS

- Security Groups
- SSH Key Authentication
- Elastic IP
- WireGuard Encryption

---

## Azure

- Network Security Groups
- SSH Key Authentication
- Static Public IP
- WireGuard Encryption

---

## Linux

- Ubuntu Server
- IP Forwarding
- WireGuard
- Public Key Authentication

---

# VPN Architecture

The VPN consists of two Linux gateways.

AWS

```
172.168.100.1
```

Azure

```
172.168.100.2
```

Each gateway exchanges only public keys and authenticates all VPN traffic using WireGuard.

---

# Verification

The completed architecture was verified through:

- Successful WireGuard tunnel establishment
- Successful VPN handshake
- AWS → Azure WireGuard connectivity
- Azure → AWS WireGuard connectivity
- AWS → Azure private network communication
- Azure → AWS private network communication

Verification screenshots are stored in:

```
images/testing/verification/
```

---

# Architecture Benefits

This design provides:

- Hybrid cloud connectivity
- Encrypted site-to-site communication
- Independent cloud environments
- Private resource communication
- Low-latency VPN connectivity
- Enterprise-ready network segmentation
- Secure remote administration
- Scalable infrastructure foundation

---

# Technologies

## Cloud

- Amazon Web Services
- Microsoft Azure

## Networking

- WireGuard
- IPv4
- TCP/IP
- Routing
- Site-to-Site VPN

## Operating System

- Ubuntu Server 24.04 LTS

## Security

- SSH Keys
- Security Groups
- Network Security Groups
- Public Key Cryptography
- ChaCha20 Encryption

---

# Skills Demonstrated

- Hybrid Cloud Architecture
- AWS Networking
- Azure Networking
- Linux Administration
- WireGuard VPN
- Network Routing
- Cloud Security
- Infrastructure Design
- Enterprise Networking
- Technical Documentation

---

# Outcome

A secure enterprise hybrid cloud architecture was successfully designed and implemented using AWS and Microsoft Azure. The completed solution establishes encrypted communication between independent cloud environments using a WireGuard site-to-site VPN while maintaining separate private address spaces, secure administrative access, and routed connectivity for future enterprise services.