# 07 - VPN Architecture

# Overview

This document describes the architecture, design decisions, and network topology of the WireGuard site-to-site VPN connecting Amazon Web Services (AWS) and Microsoft Azure within the Enterprise Hybrid Cloud Platform.

The VPN establishes a secure, encrypted connection between two independent cloud environments, allowing private resources in each network to communicate over the Internet while maintaining network isolation and strong cryptographic authentication.

---

# Objectives

- Connect AWS and Azure using a secure site-to-site VPN.
- Encrypt all inter-cloud traffic.
- Route private network traffic through the VPN.
- Maintain separate private address spaces.
- Simulate an enterprise hybrid cloud environment.
- Provide a scalable foundation for future infrastructure services.

---

# Why WireGuard?

WireGuard was selected because it provides:

- Modern cryptography
- High performance
- Lightweight implementation
- Simple configuration
- Small codebase
- Cross-platform compatibility
- Low latency

Compared to traditional VPN technologies, WireGuard requires fewer configuration files while providing strong encryption and excellent performance.

---

# Hybrid Cloud Architecture

```
                              Internet
                                  │
                    ─────────────────────────────
                                  │
                 AWS Elastic IP         Azure Static Public IP
                        │                     │
          ┌────────────────────┐   ┌────────────────────┐
          │ AWS WireGuard VM   │===│ Azure WireGuard VM │
          │ Ubuntu 24.04 LTS   │   │ Ubuntu 24.04 LTS   │
          │                    │   │                    │
          │ WG:172.168.100.1   │   │ WG:172.168.100.2   │
          └────────────────────┘   └────────────────────┘
                    │                         │
             AWS VPC                     Azure VNet
             10.0.0.0/16                10.1.0.0/16
                    │                         │
             Private Resources         Private Resources
```

---

# Network Addressing

## AWS

| Network | Address |
|----------|---------|
| VPC | 10.0.0.0/16 |
| Public Subnet | 10.0.1.0/24 |
| WireGuard Tunnel | 172.168.100.1/24 |

---

## Azure

| Network | Address |
|----------|---------|
| VNet | 10.1.0.0/16 |
| Public Subnet | 10.1.1.0/24 |
| WireGuard Tunnel | 172.168.100.2/24 |

---

## Tunnel Network

| Network | Address |
|----------|---------|
| WireGuard VPN | 172.168.100.0/24 |

---

# Traffic Flow

When an AWS resource communicates with an Azure resource:

1. Traffic destined for the Azure VNet (10.1.0.0/16) is routed to the WireGuard interface (`wg0`).
2. WireGuard encrypts the packet.
3. The encrypted packet traverses the public Internet.
4. The Azure WireGuard gateway decrypts the packet.
5. The packet is forwarded to the Azure Virtual Network.

The reverse process occurs for traffic originating from Azure.

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

Each gateway routes only the remote private network through the VPN.

Internet-bound traffic remains local to its respective cloud environment.

---

# WireGuard Tunnel

## AWS

| Setting | Value |
|----------|-------|
| Interface | wg0 |
| Tunnel Address | 172.168.100.1/24 |
| Listen Port | 51820 |

---

## Azure

| Setting | Value |
|----------|-------|
| Interface | wg0 |
| Tunnel Address | 172.168.100.2/24 |
| Listen Port | 51820 |

---

# Allowed IPs

AWS

```
AllowedIPs = 10.1.0.0/16,172.168.100.2/32
```

Azure

```
AllowedIPs = 10.0.0.0/16,172.168.100.1/32
```

The `AllowedIPs` parameter performs two functions:

- Defines which traffic should traverse the VPN tunnel.
- Identifies which remote networks are reachable through each peer.

---

# Security Architecture

The VPN uses multiple layers of security.

## Cloud Security

AWS

- Security Groups
- Elastic IP
- SSH key authentication

Azure

- Network Security Groups
- Static Public IP
- SSH key authentication

---

## WireGuard Security

- Public/Private Key Cryptography
- ChaCha20 Encryption
- Curve25519 Key Exchange
- Authenticated Peers
- UDP Port 51820

Private keys remain only on their respective VPN gateways and are never stored in the GitHub repository.

---

# Routing vs NAT

This implementation uses routed networking instead of Network Address Translation (NAT).

Reasons include:

- Non-overlapping private address spaces
- Preservation of original source IP addresses
- Simpler troubleshooting
- Better logging and auditing
- Enterprise site-to-site VPN best practices

No address translation is required because:

AWS

```
10.0.0.0/16
```

Azure

```
10.1.0.0/16
```

These networks do not overlap.

---

# High-Level Communication

```
AWS EC2
10.0.1.40
      │
      ▼
WireGuard Gateway
172.168.100.1
      │
Encrypted Tunnel
      │
172.168.100.2
WireGuard Gateway
      ▼
Azure VM
10.1.1.4
```

---

# Verification

The VPN architecture was validated through successful testing.

Verified:

- WireGuard interface established
- Peer authentication
- Successful VPN handshake
- Encrypted packet transfer
- AWS WireGuard → Azure WireGuard
- Azure WireGuard → AWS WireGuard
- AWS private network → Azure private network
- Azure private network → AWS private network

Verification screenshots are available in:

```
images/testing/verification/
```

---

# Design Decisions

The following architectural decisions were made during implementation.

- Ubuntu Server used as the VPN gateway in both cloud providers.
- Static public IPs used for consistent VPN endpoints.
- Separate private address spaces assigned to AWS and Azure.
- Dedicated WireGuard tunnel network.
- Routed VPN architecture without NAT.
- Public/private key authentication.
- Cloud-native firewall controls.

---

# Benefits

This architecture provides:

- Secure encrypted communication
- Cloud-to-cloud connectivity
- Low-latency VPN communication
- Private resource access
- Simple management
- High performance
- Enterprise-ready hybrid cloud foundation

---

# Future Enhancements

- High Availability WireGuard gateways
- Dynamic routing using BGP
- Multi-region VPN connectivity
- Cloudflare Zero Trust integration
- Active Directory integration
- Private DNS between cloud providers
- Infrastructure as Code (Terraform)
- VPN monitoring and alerting

---

# Outcome

A secure WireGuard site-to-site VPN architecture was successfully designed and implemented between AWS and Azure.

The completed solution enables encrypted communication between both cloud providers while preserving independent private networks through routed connectivity. This architecture establishes the networking foundation for deploying additional enterprise services across the hybrid cloud environment.