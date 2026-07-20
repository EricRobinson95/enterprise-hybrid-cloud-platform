# 07 - VPN Architecture

# Overview

The Enterprise Hybrid Cloud Platform uses a **WireGuard hub-and-spoke VPN architecture** to securely connect Amazon Web Services (AWS), Microsoft Azure, and an on-premises security lab.

AWS functions as the central VPN hub, providing secure encrypted connectivity between all connected environments while hosting the production web application.

This architecture simulates a real-world hybrid cloud deployment by separating production workloads, enterprise IT services, and security operations into dedicated environments connected through encrypted tunnels.

---

# VPN Objectives

- Secure communication across public networks
- Encrypt inter-site traffic
- Connect AWS, Azure, and On-Premises
- Centralize VPN management
- Support enterprise hybrid cloud networking
- Provide a scalable architecture for future expansion

---

# VPN Topology

```
                         Internet
                             │
                    Encrypted WireGuard
                             │
                             ▼
                    AWS WireGuard Hub
                     172.16.100.1
                    10.0.1.40
                     /         \
                    /           \
                   /             \
                  ▼               ▼

      Azure WireGuard      On-Premises WireGuard
          Gateway                Gateway
      172.16.100.2          172.16.100.3
        10.1.1.4              10.2.1.12
```

---

# Hybrid Cloud Topology

```
                    Internet
                        │
                  Cloudflare DNS
                        │
                        ▼
                AWS Production
                  10.0.0.0/16
         WireGuard VPN Hub
      React • FastAPI • PostgreSQL
                 /          \
                /            \
               ▼              ▼

 Azure Corporate IT      On-Premises Security Lab
     10.1.0.0/16             10.2.0.0/16

 Active Directory           Kali Linux
 Windows Server             Security Testing
 Windows 11 VM
```

---

# VPN Components

## AWS WireGuard Hub

Role

- Central VPN Hub
- Packet Routing
- VPN Termination
- Production Network Connectivity

Network

```
10.0.0.0/16
```

Tunnel Address

```
172.16.100.1
```

Responsibilities

- Accept VPN connections
- Route traffic
- Encrypt traffic
- Forward packets
- Connect Azure and On-Premises to AWS resources

---

## Azure WireGuard Gateway

Role

Securely connects the Azure corporate environment to AWS.

Network

```
10.1.0.0/16
```

Tunnel Address

```
172.16.100.2
```

Connected Services

- Windows Server 2025
- Active Directory
- DNS
- Windows 11 Developer Workstation
- Windows File Server
- Internal Application Server

---

## On-Premises WireGuard Gateway

Role

Securely connects the security operations lab to AWS.

Network

```
10.2.0.0/16
```

Tunnel Address

```
172.16.100.3
```

Connected Services

- Kali Linux
- Security Test Workstation

---

# Tunnel Addressing

| Device | Tunnel Address |
|----------|----------------|
| AWS Hub | 172.16.100.1 |
| Azure Gateway | 172.16.100.2 |
| On-Premises Gateway | 172.16.100.3 |

Tunnel Network

```
172.16.100.0/24
```

---

# Site Networks

| Environment | Network |
|------------|----------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |

---

# VPN Routing

AWS functions as the central routing hub.

Traffic flow

```
Azure

↓

WireGuard Tunnel

↓

AWS Hub

↓

Production Services
```

```
On-Premises

↓

WireGuard Tunnel

↓

AWS Hub

↓

Production Services
```

---

# Advertised Networks

## AWS

```
10.0.0.0/16
```

---

## Azure

```
10.1.0.0/16
```

---

## On-Premises

```
10.2.0.0/16
```

---

# Encryption

WireGuard provides:

- Modern Cryptography
- Public Key Authentication
- Encrypted UDP Transport
- Low Overhead
- High Performance

Each VPN peer authenticates using its own public/private key pair.

---

# Routing

Linux static routing is used on all VPN gateways.

Routes are installed through the WireGuard interface.

Example

```
10.0.0.0/16 dev wg0

10.1.0.0/16 dev wg0

10.2.0.0/16 dev wg0

172.16.100.0/24 dev wg0
```

IP forwarding is enabled on every gateway.

---

# Firewall Configuration

VPN traffic is protected using:

AWS

- Security Groups

Azure

- Network Security Groups

Linux

- UFW / iptables

WireGuard

- UDP Port 51820

---

# Verification

The following validation has been completed.

## VPN Handshakes

Verified using

```bash
sudo wg
```

Completed

- AWS ↔ Azure
- AWS ↔ On-Premises

---

## Routing

Verified using

```bash
ip route
```

Completed

- Static Routes
- WireGuard Routes

---

## IP Forwarding

Verified using

```bash
sysctl net.ipv4.ip_forward
```

Expected

```
net.ipv4.ip_forward = 1
```

---

## Packet Capture

Verified using

```bash
sudo tcpdump
```

Captured

- WireGuard UDP
- ICMP
- Tunnel Traffic

---

# Current Implementation

## Completed

- AWS WireGuard Hub
- Azure WireGuard Gateway
- On-Premises WireGuard Gateway
- Hub-and-Spoke VPN
- Static Routing
- Linux IP Forwarding
- VPN Validation
- Architecture Documentation

---

# Current Limitation

The VPN currently provides secure communication between:

- AWS ↔ Azure
- AWS ↔ On-Premises

The project uses AWS as the central VPN hub. Direct Azure ↔ On-Premises transit routing has not been implemented and is planned as a future enhancement.

This does not impact the current architecture because:

- Azure hosts enterprise identity and developer services.
- AWS hosts the production application.
- On-Premises performs security testing against AWS-hosted resources.

---

# Future Enhancements

Future improvements may include:

- High Availability WireGuard Gateways
- Dynamic Routing (BGP)
- Additional Branch Offices
- VPN Monitoring
- Infrastructure as Code (Terraform)
- Automated VPN Deployment
- Microsoft Entra ID Integration
- AWS IAM Identity Center Integration

---

# Summary

The Enterprise Hybrid Cloud Platform uses a WireGuard hub-and-spoke VPN architecture to securely connect AWS, Azure, and an on-premises security lab.

AWS serves as the central VPN hub, providing encrypted connectivity between environments while supporting enterprise identity services, production application hosting, and security testing. The architecture establishes a secure and scalable networking foundation for the remaining phases of the project.