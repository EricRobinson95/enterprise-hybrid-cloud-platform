# 05 - AWS Network

# Overview

This document describes the Amazon Web Services (AWS) networking infrastructure used within the Enterprise Hybrid Cloud Platform. The AWS environment provides secure cloud networking, internet connectivity, and one endpoint of the WireGuard site-to-site VPN connecting AWS and Microsoft Azure.

---

# Objectives

- Build a secure AWS Virtual Private Cloud (VPC).
- Create public and private subnets.
- Configure Internet connectivity.
- Deploy an Ubuntu WireGuard gateway.
- Secure resources using Security Groups.
- Integrate AWS with Azure through a WireGuard site-to-site VPN.

---

# AWS Architecture

The AWS environment consists of:

- Virtual Private Cloud (VPC)
- Public Subnet
- Internet Gateway
- Route Tables
- Security Groups
- Elastic IP Address
- Ubuntu Server 24.04 LTS EC2 Instance
- WireGuard VPN Gateway

---

# Virtual Private Cloud (VPC)

| Setting | Value |
|----------|-------|
| CIDR Block | 10.0.0.0/16 |
| DNS Resolution | Enabled |
| DNS Hostnames | Enabled |

---

# Subnets

## Public Subnet

| Setting | Value |
|----------|-------|
| CIDR Block | 10.0.1.0/24 |
| Purpose | Internet-facing resources |
| Auto Assign Public IP | Enabled |

The WireGuard gateway resides within the public subnet and uses an Elastic IP for secure remote access and VPN connectivity.

---

# Internet Gateway

An Internet Gateway (IGW) is attached to the VPC to provide inbound and outbound Internet access.

Route Table:

| Destination | Target |
|-------------|--------|
| 0.0.0.0/0 | Internet Gateway |

---

# Route Tables

The public route table includes:

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | Internet Gateway |

Additional routing for Azure private networks is handled by the WireGuard VPN.

---

# Security Groups

The WireGuard gateway uses a Security Group configured with the following inbound rules.

| Protocol | Port | Purpose |
|----------|------|---------|
| SSH | 22 | Remote administration |
| UDP | 51820 | WireGuard VPN |

Outbound traffic is allowed to all destinations.

---

# Elastic IP

A static Elastic IP is associated with the EC2 instance.

Benefits include:

- Persistent public endpoint
- Stable VPN endpoint
- Reliable SSH connectivity
- No IP changes after reboot

---

# EC2 Instance

| Setting | Value |
|----------|-------|
| Operating System | Ubuntu Server 24.04 LTS |
| Instance Role | WireGuard Gateway |
| Placement | Public Subnet |

The EC2 instance acts as the AWS VPN gateway for all encrypted communication with Azure.

---

# WireGuard VPN Integration

The AWS gateway establishes a secure encrypted tunnel with Microsoft Azure.

## WireGuard Interface

| Setting | Value |
|----------|-------|
| Interface | wg0 |
| Tunnel Address | 172.168.100.1/24 |
| Listen Port | 51820 |

## Routed Networks

| Destination | Route |
|-------------|-------|
| Azure VNet | 10.1.0.0/16 |
| Azure WireGuard Peer | 172.168.100.2/32 |

The WireGuard configuration routes Azure network traffic through the encrypted VPN tunnel.

---

# Network Diagram

```text
                 Internet
                     │
             Elastic IP Address
                     │
          Ubuntu WireGuard Gateway
               10.0.1.40
          WireGuard: 172.168.100.1
                     │
══════════════════════════════════════
      Encrypted WireGuard VPN Tunnel
══════════════════════════════════════
                     │
            Azure WireGuard Gateway
```

---

# Connectivity Verification

The AWS environment successfully communicated with:

- Azure WireGuard interface
- Azure private virtual network
- Azure Ubuntu virtual machine

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

- VPC network isolation
- Security Groups
- SSH authentication using key pairs
- Elastic IP for stable VPN endpoint
- Encrypted WireGuard tunnel
- Public key cryptography
- Linux IP forwarding

---

# Skills Demonstrated

- Amazon VPC
- EC2 Administration
- Ubuntu Linux
- Elastic IP Management
- Security Groups
- Route Tables
- Internet Gateway
- WireGuard VPN
- Hybrid Cloud Networking
- Network Verification

---

# Outcome

A secure AWS networking environment was successfully deployed to support a hybrid cloud architecture. The AWS EC2 instance functions as the WireGuard VPN gateway, providing encrypted connectivity to Microsoft Azure while maintaining secure access to private AWS resources over the site-to-site VPN.