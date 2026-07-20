# 08 - WireGuard Site-to-Site VPN

## Overview

The Enterprise Hybrid Cloud Platform uses a **WireGuard hub-and-spoke topology** to securely connect Amazon Web Services (AWS), Microsoft Azure, and an on-premises security lab.

AWS serves as the central VPN hub, allowing Azure and the on-premises environment to establish encrypted site-to-site VPN tunnels.

---

# Objectives

- Secure communication between cloud environments
- Encrypt all traffic traversing the public Internet
- Centralize routing through AWS
- Simulate a real-world hybrid cloud deployment
- Support future enterprise services such as Active Directory and production application hosting

---

# VPN Topology

```
                    Internet
                        │
                        │
                Encrypted WireGuard
                        │
                        ▼
                AWS WireGuard Hub
               172.16.100.1/24
                      /     \
                     /       \
                    /         \
                   ▼           ▼

Azure Gateway      On-Premises Gateway
172.16.100.2       172.16.100.3
```

---

# Site Networks

| Site | Network |
|-------|---------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# Hub-and-Spoke Design

AWS functions as the VPN hub.

Azure and the on-premises environment establish encrypted tunnels directly to AWS.

Current VPN topology:

```
Azure
   │
WireGuard
   │
AWS Hub
   │
WireGuard
   │
On-Premises
```

This design provides:

- Centralized routing
- Simplified management
- Reduced VPN configuration
- Easy expansion for future branch offices

---

# AWS WireGuard Hub

Role:

- Central VPN gateway
- Routes traffic between connected sites
- Terminates all VPN tunnels

Advertised Networks

```
10.0.0.0/16
10.1.0.0/16
10.2.0.0/16
172.16.100.0/24
```

Responsibilities

- Forward VPN traffic
- Maintain routing tables
- Encrypt and decrypt WireGuard traffic
- Serve as the central connectivity point

---

# Azure WireGuard Gateway

Role

Provides secure connectivity between Azure corporate infrastructure and AWS.

Connected Resources

- Windows Server 2025
- Active Directory Domain Services
- DNS
- Windows 11 Developer Workstation
- Windows File Server
- Internal Application Server

Advertised Network

```
10.1.0.0/16
```

---

# On-Premises WireGuard Gateway

Role

Provides secure connectivity between the on-premises security lab and AWS.

Connected Resources

- Kali Linux
- Security Testing Workstation

Advertised Network

```
10.2.0.0/16
```

---

# Routing

Each WireGuard gateway advertises its local subnet.

| Gateway | Advertised Networks |
|----------|---------------------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |

Linux routing tables forward traffic through the WireGuard interface for remote networks.

---

# Security

WireGuard provides:

- Modern cryptography
- Mutual peer authentication
- Minimal attack surface
- Encrypted site-to-site communication
- UDP-based transport

Each peer authenticates using a unique public/private key pair.

---

# Verification

The following tests were successfully completed.

## WireGuard Handshakes

- AWS ↔ Azure
- AWS ↔ On-Premises

Verified using:

```bash
sudo wg
```

---

## Tunnel Status

Verified

- Latest handshake
- Transfer statistics
- Connected peers

---

## Routing Verification

Verified using:

```bash
ip route
```

and

```bash
ip route get <destination>
```

---

## Traffic Capture

Traffic verification performed using:

```bash
sudo tcpdump
```

Observed

- Encrypted UDP traffic
- Tunnel encapsulation
- ICMP testing
- Route verification

---

# Current Implementation

Completed

- AWS WireGuard Hub
- Azure WireGuard Gateway
- On-Premises WireGuard Gateway
- Hub-and-spoke topology
- Linux routing
- IP forwarding
- Tunnel verification
- Architecture documentation

---

# Current Limitation

The current deployment successfully establishes secure VPN connectivity between:

- AWS ↔ Azure
- AWS ↔ On-Premises

The architecture currently relies on AWS as the central hub for inter-site communication.

Direct Azure ↔ On-Premises transit routing remains a future enhancement and is not required for the current project objectives.

---

# Future Enhancements

Planned improvements include:

- Additional branch office VPN connections
- High-availability WireGuard gateways
- Dynamic routing (BGP)
- Microsoft Entra ID integration
- AWS IAM Identity Center integration
- Centralized VPN monitoring
- Automated WireGuard deployment
- Infrastructure as Code (Terraform)

---

# Summary

The Enterprise Hybrid Cloud Platform implements a secure WireGuard hub-and-spoke VPN architecture connecting AWS, Azure, and an on-premises security lab.

AWS serves as the central VPN hub, enabling secure encrypted communication between environments while providing a scalable foundation for future enterprise services, application deployment, and cybersecurity testing.