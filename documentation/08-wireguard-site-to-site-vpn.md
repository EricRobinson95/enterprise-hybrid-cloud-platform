# 08 - WireGuard Site-to-Site VPN

# Overview

This document describes the implementation of the WireGuard site-to-site VPN used throughout the Enterprise Hybrid Cloud Platform.

WireGuard provides secure Layer 3 connectivity between Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware enterprise environment using a hub-and-spoke topology.

AWS functions as the central VPN hub while Azure and the on-premises environment operate as spoke sites. The VPN creates a single private enterprise network that supports Active Directory, enterprise DNS, Windows authentication, secure administration, and future production application services.

---

# Design Goals

The WireGuard deployment provides:

- Secure hybrid cloud connectivity
- Encrypted site-to-site communication
- Enterprise routing
- Cross-site Active Directory authentication
- Cross-site DNS resolution
- Secure remote administration
- Linux routing
- Enterprise scalability

---

# VPN Topology

```
                           Internet
                               │
                    Encrypted WireGuard VPN
                               │
                     AWS WireGuard Hub
                      172.16.100.1
                     AWS 10.0.0.0/16
                               │
              ┌────────────────┴────────────────┐
              │                                 │
              ▼                                 ▼

      Azure WireGuard Gateway          On-Premises WireGuard Gateway
          172.16.100.2                     172.16.100.3

         Azure 10.1.0.0/16               VMware 10.2.0.0/16
```

---

# Enterprise Networks

| Environment | Network |
|-------------|-------------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# Tunnel Addresses

| Gateway | Tunnel Address |
|----------|----------------|
| AWS Hub | 172.16.100.1 |
| Azure Gateway | 172.16.100.2 |
| On-Premises Gateway | 172.16.100.3 |

---

# WireGuard Architecture

## AWS Hub

Interfaces

```
eth0
10.0.1.40

wg0
172.16.100.1
```

Responsibilities

- VPN Hub
- Linux Routing
- Packet Forwarding
- Static Routing
- SSH Administration

Connected Peers

- Azure
- On-Premises

---

## Azure Gateway

Interfaces

```
eth0
10.1.1.4

wg0
172.16.100.2
```

Responsibilities

- VPN Spoke
- Linux Routing
- Azure Enterprise Connectivity

Connected Peer

- AWS Hub

---

## On-Premises Gateway

Interfaces

```
eth0
10.2.1.12

wg0
172.16.100.3
```

Responsibilities

- VPN Spoke
- Linux Routing
- VMware Connectivity

Connected Peer

- AWS Hub

---

# Allowed Networks

## AWS Hub

```
10.1.0.0/16
10.2.0.0/16
172.16.100.2/32
172.16.100.3/32
```

---

## Azure

```
10.0.0.0/16
10.2.0.0/16
172.16.100.1/32
172.16.100.3/32
```

---

## On-Premises

```
10.0.0.0/16
10.1.0.0/16
172.16.100.1/32
172.16.100.2/32
```

---

# Enterprise Routing

WireGuard provides encrypted transport only.

Routing is provided by:

AWS

- VPC Route Tables
- Linux Static Routes

Azure

- User Defined Routes
- Linux Static Routes

On-Premises

- Linux Static Routes
- Windows Persistent Routes

---

# Linux Configuration

All WireGuard gateways perform Layer 3 routing.

## IP Forwarding

```
net.ipv4.ip_forward=1
```

Verification

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Expected Output

```
1
```

---

## Firewall

Traffic forwarding is permitted through iptables.

Example

```bash
iptables -A FORWARD -i wg0 -j ACCEPT
iptables -A FORWARD -o wg0 -j ACCEPT
```

---

## NAT

Internet-bound traffic is translated using MASQUERADE.

```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

---

# Azure Networking

Azure required additional routing configuration.

Implemented

- User Defined Routes
- Route Table Associations
- Network Security Groups
- IP Forwarding

The Azure route table is associated with:

- Gateway Subnet
- Identity Services Subnet

This allows Windows Server 2025 to communicate with AWS and the on-premises enterprise network.

---

# AWS Configuration

AWS required:

- Source/Destination Check disabled
- Linux IP Forwarding
- Static Routes
- WireGuard Hub

---

# On-Premises Configuration

Implemented

- Ubuntu WireGuard Gateway
- Linux Static Routes
- Windows Persistent Routes
- VMware Networking

---

# Verification Commands

WireGuard

```bash
wg
```

Routes

```bash
ip route
```

Interfaces

```bash
ip addr
```

IP Forwarding

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Firewall

```bash
iptables -L

iptables -t nat -L
```

Packet Capture

```bash
tcpdump -ni wg0
```

Windows

```powershell
route print

tracert

ping

nslookup
```

---

# Validation

The VPN infrastructure has been fully validated.

## WireGuard

Verified

- Peer Handshakes
- Tunnel Status

---

## Routing

Verified

- Linux Routing
- AWS Route Tables
- Azure User Defined Routes
- Windows Persistent Routes

---

## Connectivity

Verified

- AWS ↔ Azure
- AWS ↔ On-Premises
- Azure ↔ On-Premises

---

## DNS

Verified

- Cross-site DNS Resolution

---

## Active Directory

Verified

- Cross-site Authentication
- Domain Login
- Enterprise Users

---

## Secure Administration

Verified

- SSH
- Windows Remote Desktop

---

# Lessons Learned

During implementation the following concepts were reinforced.

- WireGuard handshakes do not guarantee end-to-end connectivity.
- `AllowedIPs` functions as both a routing table and an access control list.
- Linux IP forwarding is required on VPN gateways.
- AWS EC2 Source/Destination Check must be disabled when acting as a router.
- Azure User Defined Routes must be associated with every subnet that requires access to remote networks.
- Windows clients require persistent routes when a cloud gateway is not their default gateway.
- Packet captures with `tcpdump` greatly simplify VPN troubleshooting.
- Systematic troubleshooting is significantly more effective than changing multiple configuration items simultaneously.

---

# Current Status

| Component | Status |
|-----------|--------|
| AWS WireGuard Hub | Complete |
| Azure WireGuard Spoke | Complete |
| On-Premises WireGuard Spoke | Complete |
| Site-to-Site VPN | Complete |
| Linux Routing | Complete |
| AWS Route Tables | Complete |
| Azure User Defined Routes | Complete |
| Windows Persistent Routes | Complete |
| IP Forwarding | Complete |
| Cross-site Routing | Complete |
| Cross-site DNS | Complete |
| Active Directory Authentication | Complete |
| Domain-Joined Windows Workstations | Complete |
| VPN Validation | Complete |

---

# Future Enhancements

Planned improvements include:

- High Availability VPN
- Dynamic Routing (BGP)
- Dual AWS VPN Hubs
- Infrastructure as Code (Terraform)
- Configuration Management (Ansible)
- Prometheus Monitoring
- Grafana Dashboards
- Automated VPN Health Checks

---

# Summary

WireGuard provides the secure networking foundation for the Enterprise Hybrid Cloud Platform by integrating AWS, Azure, and the on-premises enterprise environment into a single private routed network.

The completed implementation includes encrypted site-to-site connectivity, Linux routing, Azure User Defined Routes, Windows persistent routes, cross-site DNS resolution, Active Directory authentication, and validated end-to-end communication between every enterprise location. This VPN infrastructure enables centralized identity management, secure administration, and future production application hosting.