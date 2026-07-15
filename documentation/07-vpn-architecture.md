# 07 - VPN Architecture

# Enterprise Hybrid Cloud Platform - VPN Architecture

## Overview

The Enterprise Hybrid Cloud Platform uses a secure site-to-site Virtual Private Network (VPN) to connect Amazon Web Services (AWS), Microsoft Azure, and the on-premises environment into a single hybrid enterprise network.

Rather than exposing internal services to the public Internet, all inter-site communication is encrypted using WireGuard VPN tunnels. This allows cloud resources and on-premises infrastructure to communicate securely over public Internet connections.

---

# Design Objectives

The VPN architecture has been designed to:

- Secure communication between cloud environments
- Connect AWS, Azure, and the on-premises lab
- Protect management traffic from public exposure
- Support centralized identity services
- Allow private administration of cloud resources
- Provide a scalable hybrid cloud foundation
- Minimize infrastructure cost while maintaining enterprise design principles

---

# VPN Topology

The platform uses a **hub-and-spoke** VPN topology.

```
                 AWS
          WireGuard Hub
           /          \
          /            \
     Azure          On-Premises
```

AWS functions as the central VPN hub.

Azure and the on-premises environment each maintain a secure WireGuard tunnel to AWS. Traffic between Azure and the on-premises environment is routed through AWS.

This design simplifies routing, reduces the number of VPN tunnels required, and provides a single location for VPN management.

---

# Why Hub-and-Spoke?

Several VPN topologies were evaluated during the design phase.

## Hub-and-Spoke (Selected)

Advantages:

- Simpler routing
- Lower operational complexity
- Fewer VPN tunnels
- Easier troubleshooting
- Lower cloud infrastructure cost
- Easier future expansion

Disadvantages:

- AWS becomes the central routing point
- Traffic between Azure and the on-premises environment traverses AWS

---

## Full Mesh (Not Selected)

A full mesh topology would provide direct communication between every site.

Advantages:

- Direct communication between all locations
- Lower latency between individual sites
- No central routing hub

Disadvantages:

- Increased configuration complexity
- More VPN tunnels
- More difficult troubleshooting
- Poor scalability as additional sites are added

For this project, the hub-and-spoke design provides the best balance between enterprise architecture, simplicity, and cost.

---

# Why WireGuard?

WireGuard was selected because it provides:

- Modern cryptography
- High performance
- Lightweight implementation
- Fast tunnel establishment
- Simple configuration
- Cross-platform support

Compared to traditional VPN technologies such as OpenVPN, WireGuard requires less configuration while providing excellent performance.

---

# VPN Gateways

Each environment contains a dedicated WireGuard gateway.

## AWS VPN Gateway

Platform:

- Ubuntu EC2 Instance

Responsibilities:

- VPN hub
- Route cloud traffic
- Encrypt and decrypt VPN traffic
- Forward traffic between AWS, Azure, and the on-premises environment

---

## Azure VPN Gateway

Platform:

- Ubuntu Virtual Machine

Responsibilities:

- Connect Azure to AWS
- Encrypt and decrypt VPN traffic
- Forward traffic into the Azure Virtual Network

---

## On-Premises VPN Gateway

Platform:

- Ubuntu Server Virtual Machine (VMware Workstation Pro)

Responsibilities:

- Connect the home lab to AWS
- Provide secure administrative access
- Route local network traffic through the VPN

The virtual gateway can later be replaced with dedicated network hardware such as a Cisco router or firewall without requiring changes to the cloud environments.

---

# Advertised Networks

Each VPN gateway advertises the private network it owns.

| Environment | Network |
|-------------|---------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |

These advertised networks allow each environment to determine where traffic should be forwarded across the VPN.

---

# Traffic Flow

## Client Traffic

Public client requests follow this path:

```
Client
    │
Cloudflare
    │
Application Load Balancer
    │
Amazon ECS
    │
Amazon RDS
```

Client traffic does **not** traverse the VPN.

---

## Hybrid Infrastructure Traffic

Administrative and infrastructure traffic follows this path:

```
AWS
     │
WireGuard Tunnel
     │
Azure
```

or

```
On-Premises
      │
WireGuard Tunnel
      │
AWS
      │
WireGuard Tunnel
      │
Azure
```

Only private infrastructure traffic uses the VPN.

---

# Routing

Routing decisions occur at multiple layers.

1. AWS Route Tables determine whether traffic should be forwarded to the WireGuard gateway.
2. Linux routing tables determine which interface forwards the packet.
3. WireGuard selects the appropriate VPN peer based on the configured AllowedIPs.
4. Azure and the on-premises environment perform similar routing decisions within their own networks.

This layered routing model ensures that private traffic remains encrypted while moving between environments.

---

# Security Considerations

The VPN architecture follows several security principles.

- Encryption of all inter-site traffic
- Least privilege routing
- Private addressing
- No direct exposure of internal services
- Dedicated VPN gateways
- Segmented cloud environments

Only management and infrastructure traffic traverses the VPN.

Public application traffic remains isolated from the management network.

---

# Future Enhancements

Future improvements include:

- High Availability VPN gateways
- Automatic failover
- Dynamic routing
- Additional branch office connectivity
- Dedicated Cisco edge router
- Physical Layer 3 switching
- Infrastructure as Code (Terraform)
- Automated VPN deployment

---

# Summary

The VPN architecture provides secure hybrid connectivity between AWS, Azure, and the on-premises environment using WireGuard. A hub-and-spoke topology was selected to simplify routing, reduce operational complexity, and provide a scalable foundation for future expansion. By separating public application traffic from private infrastructure traffic, the design follows modern enterprise networking best practices while supporting secure administration across all environments.