# 06 - Azure Network

# Overview

This document describes the Microsoft Azure networking infrastructure used within the Enterprise Hybrid Cloud Platform. The Azure environment provides secure cloud networking, internet connectivity, and one endpoint of the WireGuard site-to-site VPN connecting Microsoft Azure and Amazon Web Services (AWS).

---

# Objectives

- Build a secure Azure Virtual Network (VNet).
- Create public and private subnets.
- Configure Internet connectivity.
- Deploy an Ubuntu WireGuard gateway.
- Secure resources using Network Security Groups (NSGs).
- Integrate Azure with AWS through a WireGuard site-to-site VPN.

---

# Azure Architecture

The Azure environment consists of:

- Virtual Network (VNet)
- Public Subnet
- Private Subnet
- Route Tables
- Network Security Group (NSG)
- Static Public IP Address
- Network Interface (NIC)
- Ubuntu Server 24.04 LTS Virtual Machine
- WireGuard VPN Gateway

---

# Virtual Network (VNet)

| Setting | Value |
|----------|-------|
| Address Space | 10.1.0.0/16 |

---

# Subnets

## Public Subnet

| Setting | Value |
|----------|-------|
| CIDR Block | 10.1.1.0/24 |
| Purpose | Internet-facing resources |

The WireGuard gateway resides within the public subnet and uses a Static Public IP for secure remote administration and VPN connectivity.

---

# Network Security Group (NSG)

The Network Security Group protects the virtual machine by allowing only required inbound traffic.

| Protocol | Port | Purpose |
|----------|------|---------|
| SSH | 22 | Remote administration |
| UDP | 51820 | WireGuard VPN |

Outbound traffic is allowed to all destinations.

---

# Static Public IP

The Azure Virtual Machine uses a Standard Static Public IP.

Benefits include:

- Permanent VPN endpoint
- Reliable SSH access
- Consistent WireGuard peer configuration
- No public IP changes after VM restart

---

# Network Interface (NIC)

The Azure VM uses a dedicated Network Interface Card (NIC) connected to the public subnet.

The NIC provides:

- Private VNet connectivity
- Public Internet access
- WireGuard VPN endpoint
- NSG association

---

# Azure Virtual Machine

| Setting | Value |
|----------|-------|
| Operating System | Ubuntu Server 24.04 LTS |
| Role | WireGuard Gateway |
| Placement | Public Subnet |

The Azure VM functions as the Azure VPN gateway for secure communication with AWS.

---

# WireGuard VPN Integration

The Azure gateway establishes a secure encrypted tunnel with AWS.

## WireGuard Interface

| Setting | Value |
|----------|-------|
| Interface | wg0 |
| Tunnel Address | 172.168.100.2/24 |
| Listen Port | 51820 |

## Routed Networks

| Destination | Route |
|-------------|-------|
| AWS VPC | 10.0.0.0/16 |
| AWS WireGuard Peer | 172.168.100.1/32 |

The WireGuard configuration routes AWS network traffic through the encrypted VPN tunnel.

---

# Network Diagram

```text
             Internet
                 │
        Azure Static Public IP
                 │
     Ubuntu WireGuard Gateway
            10.1.1.4
      WireGuard: 172.168.100.2
                 │
══════════════════════════════════════
     Encrypted WireGuard VPN Tunnel
══════════════════════════════════════
                 │
        AWS WireGuard Gateway
```

---

# Connectivity Verification

The Azure environment successfully communicated with:

- AWS WireGuard interface
- AWS private virtual network
- AWS Ubuntu EC2 instance

Verification completed using:

- WireGuard handshake
- ICMP testing
- Private network routing
- End-to-end VPN validation

See:

```text
documentation/09-wireguard-site-to-site-vpn.md
```

---

# Security Features

- Azure Virtual Network isolation
- Network Security Groups
- SSH authentication using key pairs
- Static Public IP
- Encrypted WireGuard tunnel
- Public key cryptography
- Linux IP forwarding

---

# Skills Demonstrated

- Microsoft Azure
- Azure Virtual Network
- Azure Virtual Machine
- Network Security Groups
- Static Public IP
- Ubuntu Linux
- WireGuard VPN
- Hybrid Cloud Networking
- Network Routing
- Infrastructure Security

---

# Outcome

A secure Azure networking environment was successfully deployed to support a hybrid cloud architecture. The Azure Virtual Machine functions as the WireGuard VPN gateway, providing encrypted connectivity to AWS while maintaining secure access to private Azure resources over the site-to-site VPN.