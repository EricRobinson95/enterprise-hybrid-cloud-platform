# Enterprise Hybrid Cloud Platform

## Project Overview

This project demonstrates the design, deployment, and documentation of a secure enterprise hybrid cloud environment connecting Amazon Web Services (AWS) and Microsoft Azure using a WireGuard site-to-site VPN.

The environment simulates a real-world enterprise network where resources hosted in separate cloud providers communicate securely over encrypted tunnels while maintaining network segmentation and private IP connectivity.

The project emphasizes cloud networking, Linux administration, VPN technologies, infrastructure documentation, and network verification while following enterprise networking and security best practices.

---

# Project Goals

- Build a secure hybrid cloud network between AWS and Azure.
- Deploy Linux virtual machines in separate cloud environments.
- Configure a WireGuard site-to-site VPN.
- Enable encrypted communication between AWS and Azure.
- Verify private network connectivity across both cloud providers.
- Document all configuration, testing, and verification procedures.
- Create a portfolio-quality enterprise networking project.

---

# Technologies Used

## Amazon Web Services (AWS)

- Amazon EC2
- Amazon VPC
- Security Groups
- Elastic IP
- Ubuntu Server 24.04 LTS

## Microsoft Azure

- Azure Virtual Machine
- Azure Virtual Network (VNet)
- Network Security Group (NSG)
- Static Public IP
- Ubuntu Server 24.04 LTS

## Networking

- WireGuard VPN
- IPv4 Routing
- Private Addressing
- Linux Networking
- Secure Shell (SSH)

## Operating System

- Ubuntu Server 24.04 LTS

## Tools

- Visual Studio Code
- Git
- GitHub
- Draw.io
- Windows Terminal

---

# Network Architecture

The environment consists of two cloud environments connected through a secure WireGuard VPN tunnel.

AWS hosts:

- Ubuntu WireGuard Gateway
- AWS VPC
- Private subnet

Azure hosts:

- Ubuntu WireGuard Gateway
- Azure Virtual Network
- Private subnet

Both gateways communicate through encrypted WireGuard tunnels using static public IP addresses while routing private network traffic through the VPN.

---

# VPN Configuration

Tunnel Network

```
172.168.100.0/24
```

AWS WireGuard Address

```
172.168.100.1
```

Azure WireGuard Address

```
172.168.100.2
```

AWS Private Network

```
10.0.0.0/16
```

Azure Private Network

```
10.1.0.0/16
```

---

# Features

- Secure WireGuard site-to-site VPN
- Encrypted inter-cloud communication
- Static public endpoints
- Private network routing
- Persistent VPN tunnel
- Linux IP forwarding
- Cloud-native firewall configuration
- Infrastructure documentation
- Configuration version control
- Network verification and testing

---

# Verification

The VPN implementation was validated by verifying:

- Successful WireGuard tunnel establishment
- Active VPN handshake
- Bidirectional WireGuard tunnel communication
- AWS to Azure private network connectivity
- Azure to AWS private network connectivity
- End-to-end ICMP testing
- Secure routing across the encrypted tunnel

Verification screenshots are available in:

```
images/testing/verification/
```

---

# Skills Demonstrated

## Cloud Computing

- AWS
- Microsoft Azure

## Networking

- VPN Technologies
- WireGuard
- Routing
- IP Addressing
- Network Segmentation
- Private Networking

## Linux Administration

- Ubuntu Server
- SSH
- System Configuration
- Network Configuration
- Service Management

## Security

- Encrypted VPN Tunnels
- Public Key Cryptography
- Firewall Configuration
- Security Groups
- Network Security Groups

## DevOps

- Git
- GitHub
- Infrastructure Documentation
- Configuration Management

---

# Future Enhancements

- Azure Active Directory integration
- Windows Server Active Directory
- DNS integration across both clouds
- File Services
- Certificate Services
- Cloudflare Zero Trust
- High Availability VPN
- Dynamic Routing (BGP)
- Monitoring and Logging
- Infrastructure as Code using Terraform

---

# Repository Structure

```
enterprise-hybrid-cloud-platform/
│
├── configs/
├── diagrams/
├── documentation/
├── images/
├── scripts/
├── LICENSE
└── README.md
```

---

# Project Status

**Current Phase:** WireGuard Site-to-Site VPN Completed

## Completed

- AWS networking
- Azure networking
- Ubuntu virtual machines
- SSH access
- WireGuard installation
- WireGuard key generation
- VPN tunnel configuration
- Secure site-to-site connectivity
- Private network routing
- End-to-end connectivity verification
- Project documentation

## Next Phase

Deploy enterprise services across the hybrid cloud environment, including Active Directory, DNS, file services, and additional infrastructure components over the established VPN.