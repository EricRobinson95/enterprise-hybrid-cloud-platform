# 03 - Network Architecture

# Enterprise Hybrid Cloud Platform - Network Architecture

## Overview

The Enterprise Hybrid Cloud Platform is a modern hybrid cloud environment designed to demonstrate enterprise networking, cloud infrastructure, cybersecurity, automation, and application deployment.

The platform integrates Amazon Web Services (AWS), Microsoft Azure, Cloudflare, and an on-premises network into a secure, scalable, and highly available architecture.

Each environment has a dedicated responsibility while securely communicating through encrypted VPN connections.

---

# Architecture Goals

The network architecture has been designed to:

- Build a production-style hybrid cloud environment
- Secure public-facing services
- Separate workloads using network segmentation
- Support cloud scalability
- Enable centralized identity management
- Provide secure remote administration
- Integrate cloud and on-premises infrastructure
- Support Infrastructure as Code (Terraform)
- Enable automated deployments through GitHub Actions

---

# High-Level Architecture

```
                     Internet
                          │
                     Cloudflare
                          │
          ┌───────────────┴───────────────┐
          │                               │
      AWS Cloud                    Azure Cloud
          │                               │
          └──────────── VPN ──────────────┘
                          │
                 On-Premises Network
```

---

# Platform Components

## Cloudflare

Cloudflare serves as the public entry point into the platform.

Responsibilities include:

- DNS
- SSL/TLS
- Web Application Firewall (WAF)
- DDoS protection
- Reverse proxy
- Content Delivery Network (CDN)

---

## Amazon Web Services (AWS)

AWS hosts the public-facing application platform.

Primary responsibilities include:

- Containerized application hosting
- Application networking
- Database services
- Monitoring
- Logging

Detailed AWS design is documented in **05-aws-network.md**.

---

## Microsoft Azure

Azure provides enterprise infrastructure services.

Primary responsibilities include:

- Microsoft Entra ID
- Active Directory
- DNS
- Windows Server
- File services
- Identity management

Detailed Azure design is documented in **06-azure-network.md**.

---

## On-Premises Network

The on-premises environment extends the cloud platform into a physical engineering lab.

Responsibilities include:

- Infrastructure testing
- Network administration
- Local services
- Hybrid connectivity
- Future virtualization

The environment will also integrate VMware Workstation Pro for virtualization and a ClockworkPi uConsole as a portable field engineering workstation.

---

# Hybrid Connectivity

AWS, Azure, and the on-premises network communicate using encrypted WireGuard VPN tunnels.

Hybrid connectivity enables:

- Secure administration
- Private resource access
- Identity integration
- Backup and recovery
- Infrastructure management

---

# Site-to-Site VPN Architecture

The Enterprise Hybrid Cloud Platform uses a secure **WireGuard site-to-site VPN** to connect AWS, Azure, and the on-premises environment into a single hybrid network.

A **hub-and-spoke** topology has been selected, with AWS serving as the central VPN hub. Azure and the on-premises environment each maintain an encrypted WireGuard tunnel to AWS. Traffic between Azure and the on-premises environment is securely routed through the AWS hub.

This design simplifies routing, reduces operational complexity, lowers infrastructure costs, and provides a scalable foundation for future expansion.

### Hybrid VPN Topology

```
                 AWS
          WireGuard Hub
           /          \
          /            \
     Azure          On-Premises
```

Additional VPN implementation details, routing design, and security considerations are documented in **07-vpn-architecture.md**.

---

# Network Design Principles

The architecture follows modern enterprise networking best practices.

Key principles include:

- Defense in depth
- Least privilege access
- Network segmentation
- Private networking by default
- High availability
- Scalability
- Secure hybrid connectivity
- Infrastructure automation

---

# Supporting Documentation

This document provides a high-level overview of the platform architecture.

Additional implementation details are documented separately:

| Document | Description |
|----------|-------------|
| 04-ip-addressing.md | Enterprise IP addressing plan |
| 05-aws-network.md | AWS infrastructure design |
| 06-azure-network.md | Azure infrastructure design |
| 07-vpn-architecture.md | WireGuard VPN architecture |

---

# Summary

The Enterprise Hybrid Cloud Platform combines public cloud services, enterprise identity, secure networking, and on-premises infrastructure into a single hybrid environment. By integrating AWS, Azure, Cloudflare, and a WireGuard-based VPN architecture, the platform demonstrates modern enterprise design principles while providing a scalable foundation for cloud engineering, networking, cybersecurity, automation, and application deployment.