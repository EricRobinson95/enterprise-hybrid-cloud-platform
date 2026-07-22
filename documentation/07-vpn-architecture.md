# VPN Architecture

## Overview

The Enterprise Hybrid Cloud Platform uses a hub-and-spoke Virtual Private Network (VPN) architecture to securely connect Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware environment.

The VPN establishes encrypted communication between all three environments, allowing cloud resources and on-premises infrastructure to operate as a single enterprise network while traversing the public Internet securely.

Rather than creating direct VPN tunnels between every site, the environment uses a centralized hub-and-spoke topology. AWS serves as the VPN hub while Azure and the on-premises environment function as spokes.

---

# Design Objectives

The VPN architecture was designed to achieve the following objectives.

- Secure communication between cloud providers
- Secure connectivity to the on-premises environment
- Centralized routing
- Reduced administrative complexity
- Simplified network expansion
- Encrypted site-to-site communication
- Secure remote administration
- Enterprise scalability

---

# VPN Topology

```
                 Azure
                   │
                   │
              VPN Tunnel
                   │
                   │
             AWS VPN Hub
                   │
                   │
              VPN Tunnel
                   │
                   │
            On-Premises
```

AWS acts as the central routing point for all VPN traffic.

Azure and the on-premises environment do not establish a direct VPN tunnel. All communication between the spoke sites traverses the AWS hub.

---

# Hub-and-Spoke Design

A hub-and-spoke architecture was selected instead of a full mesh VPN.

### Advantages

- Centralized routing
- Simplified VPN management
- Fewer VPN tunnels
- Easier troubleshooting
- Lower administrative overhead
- Scalable architecture
- Consistent security policies

As additional cloud providers or branch offices are added, only a single VPN connection to the AWS hub is required.

---

# AWS as the Hub

AWS hosts the public-facing application infrastructure for the Enterprise Hybrid Cloud Platform.

Placing the VPN hub within AWS allows Azure identity services and the on-premises environment to securely access cloud-hosted resources through a centralized gateway.

The AWS hub is responsible for forwarding traffic between all connected networks while maintaining encrypted communication across the VPN.

---

# Azure as a Spoke

Azure provides enterprise identity and infrastructure services.

Current services include

- Active Directory Domain Services
- DNS
- Identity Management

Future services include

- Windows File Server
- Internal Application Server
- Windows 11 Enterprise Client

Azure securely communicates with AWS and the on-premises environment through the VPN hub.

---

# On-Premises as a Spoke

The on-premises environment represents a traditional enterprise office or data center.

Current resources include

- Ubuntu WireGuard Gateway
- VMware Infrastructure
- Kali Linux Security Workstation

Future resources include

- Windows 11 Enterprise Client

The on-premises environment securely accesses cloud resources through the AWS VPN hub.

---

# Security Architecture

The VPN provides secure communication between all connected environments.

Security objectives include

- Confidentiality
- Integrity
- Authentication
- Secure key exchange
- Protection of internal traffic across the public Internet

Administrative access to cloud resources is performed using secure SSH connections.

---

# Traffic Flow

The VPN supports communication between

- AWS and Azure
- AWS and On-Premises
- Azure and On-Premises

Traffic between Azure and the on-premises environment is routed through the AWS hub.

This centralized routing model simplifies administration while maintaining secure communication between all enterprise locations.

---

# Scalability

The hub-and-spoke architecture allows additional environments to be integrated with minimal changes.

Examples include

- Additional AWS Regions
- Additional Azure Regions
- Branch Offices
- Remote Users
- Disaster Recovery Sites
- Additional Cloud Providers

Each new environment requires only a single VPN connection to the AWS hub.

---

# High Availability

The current implementation uses a single VPN hub located in AWS.

Future enhancements may include

- Redundant VPN gateways
- Multi-region deployment
- Automatic failover
- Dynamic routing protocols
- High availability cloud networking

---

# Benefits

The selected architecture provides

- Secure hybrid cloud networking
- Centralized VPN management
- Reduced operational complexity
- Enterprise scalability
- Secure cloud integration
- Simplified network administration
- Secure communication across public networks

---

# Current Status

The VPN architecture has been successfully implemented.

Completed

- AWS VPN Hub
- Azure VPN Spoke
- On-Premises VPN Spoke
- Hub-and-Spoke Topology
- Secure Site-to-Site Connectivity
- Cross-Site Routing
- End-to-End Communication

The VPN architecture now provides secure communication between AWS, Azure, and the on-premises environment and serves as the networking foundation for the remainder of the Enterprise Hybrid Cloud Platform.