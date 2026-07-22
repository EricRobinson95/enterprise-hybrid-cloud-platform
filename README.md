# Enterprise Hybrid Cloud Platform

## Project Overview

This project demonstrates the design and implementation of a secure enterprise hybrid cloud environment connecting Amazon Web Services (AWS), Microsoft Azure, and an on-premises network using WireGuard VPN.

The environment simulates a real-world enterprise network where cloud resources and on-premises infrastructure communicate securely over encrypted tunnels using a hub-and-spoke topology.

The goal of this project is to showcase practical cloud networking, Linux administration, Windows Server administration, routing, VPN technologies, and infrastructure documentation.

---

## Architecture

```
                 Internet
                     │
                     │
              AWS WireGuard Hub
             (Ubuntu 26.04 LTS)
              172.16.100.1
                     │
         ┌───────────┴───────────┐
         │                       │
         │                       │
Azure WireGuard             On-Premises
Ubuntu Gateway              WireGuard Gateway
172.16.100.2                172.16.100.3
         │                       │
         │                       │
 Azure VNet               VMware Network
 10.1.0.0/16              10.2.0.0/16
```

---

# Technologies Used

- Amazon Web Services (AWS)
- Microsoft Azure
- VMware Workstation Pro
- Ubuntu Server 26.04 LTS
- Windows Server 2025
- WireGuard VPN
- Linux Routing
- iptables
- TCPDump
- SSH
- PowerShell
- Draw.io
- Git
- GitHub

---

# Current Project Status

## Completed

### AWS

- AWS VPC deployment
- Ubuntu WireGuard gateway
- Security Groups configured
- Source/Destination Check disabled
- IP forwarding enabled
- iptables forwarding configured
- NAT configured
- Static routing configured
- SSH administration configured

---

### Microsoft Azure

- Azure Virtual Network
- Ubuntu WireGuard gateway
- User Defined Routes (UDRs)
- Network Security Groups
- Linux routing configured
- WireGuard configured
- End-to-end VPN connectivity verified

---

### On-Premises

- Ubuntu WireGuard gateway running in VMware
- Static routing configured
- Linux IP forwarding enabled
- SSH enabled
- VMware networking configured

---

### Windows Infrastructure

Completed:

- Windows Server 2025 deployed in Azure
- Active Directory Domain Services installed
- Domain created
- Organizational Units created
- User accounts created
- Security Groups created
- User assigned to appropriate group

---

### WireGuard VPN

Successfully implemented a working Hub-and-Spoke VPN topology.

Hub

- AWS WireGuard Gateway

Spokes

- Azure WireGuard Gateway
- On-Premises WireGuard Gateway

Verified:

- WireGuard handshakes
- Site-to-site routing
- End-to-end connectivity
- Encrypted communication
- Inter-site ICMP communication

---

### Verification Completed

Verified with:

- ping
- wg
- ip route
- tcpdump
- SSH

Connectivity verified between

- AWS ↔ Azure
- AWS ↔ On-Premises
- Azure ↔ On-Premises

---

# Remaining Work

## Azure

- Deploy Windows 11 Client
- Join Windows 11 to Active Directory domain
- Test Group Policy
- Verify authentication

---

## On-Premises

- Deploy Windows 11 Client
- Join Windows 11 to Active Directory domain
- Verify cross-site authentication
- Verify DNS resolution
- Test access across VPN

---

## Documentation

Still to complete:

- Detailed deployment guide
- WireGuard configuration guide
- Active Directory documentation
- Routing documentation
- Troubleshooting guide
- Network verification report

---

# Skills Demonstrated

## Cloud

- AWS
- Azure
- Hybrid Cloud Networking

## Networking

- Routing
- VPN
- Hub-and-Spoke Design
- Static Routes
- Linux Networking
- TCP/IP
- SSH

## Security

- WireGuard VPN
- Firewall Configuration
- Network Segmentation
- Secure Remote Administration

## Windows Administration

- Active Directory
- Organizational Units
- Users
- Groups
- Windows Server 2025

## Linux Administration

- Ubuntu Server
- IP Forwarding
- iptables
- Routing
- SSH
- WireGuard
- TCPDump

---

# Future Improvements

- High Availability WireGuard
- BGP Dynamic Routing
- DNS Integration
- Azure Bastion
- AWS Systems Manager
- Monitoring
- Grafana
- Prometheus
- Ansible Automation
- Terraform Infrastructure as Code
- CI/CD Deployment

---

# Repository Structure

```
configs/
documentation/
diagrams/
images/
scripts/
```

---

# Project Outcome

This project successfully demonstrates a production-style hybrid cloud environment connecting AWS, Azure, and an on-premises network through a secure WireGuard hub-and-spoke VPN.

The environment provides encrypted communication between all sites while showcasing practical enterprise networking, Linux administration, Windows Server administration, cloud networking, and infrastructure documentation.

Current Progress: **~90% Complete**

Remaining work consists primarily of deploying Windows 11 clients, joining them to the Active Directory domain, validating Group Policy and authentication across sites, and completing project documentation.