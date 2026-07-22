# AWS Network 

## Overview

AWS serves as the central cloud environment and WireGuard hub for the Enterprise Hybrid Cloud Platform. It hosts the public-facing application infrastructure and securely connects Azure and the on-premises environment using a hub-and-spoke VPN topology.

Responsibilities include:

- Public application hosting
- WireGuard VPN hub
- Amazon ECS application platform
- Amazon RDS PostgreSQL database
- Application Load Balancer
- Cloudflare DNS integration
- Secure routing between all connected networks

---

# AWS Architecture

```
Internet
    │
Cloudflare DNS
    │
Internet Gateway
    │
AWS VPC (10.0.0.0/16)
    │
Application Load Balancer
    │
Amazon ECS
    │
Amazon RDS PostgreSQL
```

The AWS WireGuard Gateway provides encrypted connectivity to Azure and the on-premises environment.

---

# Virtual Private Cloud

| Property | Value |
|----------|-------|
| VPC CIDR | 10.0.0.0/16 |
| Region | (Your AWS Region) |
| Internet Gateway | Attached |
| Availability Zones | 1 (Current) |

---

# Subnet Design

## Public Subnet

```
10.0.1.0/24
```

Purpose

- Internet Gateway
- Application Load Balancer
- Ubuntu WireGuard Gateway

Current Resources

- AWS Internet Gateway
- Ubuntu EC2 WireGuard VPN Gateway
- Application Load Balancer

---

## Private Application Subnet

```
10.0.2.0/24
```

Purpose

- Amazon ECS Cluster
- Backend Containers
- Internal Application Services

Current Resources

- Amazon ECS Cluster

---

## Private Database Subnet

```
10.0.3.0/24
```

Purpose

- Amazon RDS PostgreSQL

Current Resources

- Amazon RDS PostgreSQL

---

# AWS WireGuard Gateway

Ubuntu EC2 Instance

Purpose

Acts as the central VPN hub for the hybrid cloud platform.

Interfaces

| Interface | Address |
|----------|----------|
| eth0 | 10.0.1.40 |
| wg0 | 172.16.100.1 |

Connected Peers

| Peer | Tunnel IP |
|-------|-----------|
| Azure | 172.16.100.2 |
| On-Premises | 172.16.100.3 |

VPN Role

- Central Hub
- Site-to-Site VPN Gateway
- Encrypted Routing
- Inter-cloud Communication

---

# Routing

AWS advertises the following remote networks through WireGuard.

| Destination | Interface |
|-------------|-----------|
| 10.1.0.0/16 | wg0 |
| 10.2.0.0/16 | wg0 |

Example Routing Table

```
10.1.0.0/16 dev wg0
10.2.0.0/16 dev wg0
```

Default Route

```
Default Gateway

10.0.1.1
```

---

# Network Connectivity

AWS can securely communicate with

- Azure Active Directory
- Azure DNS
- Azure Windows Server 2025
- Azure File Server (planned)
- Azure Internal Application Server (planned)
- Azure Windows 11 Client (planned)

and

- Kali Linux
- Windows 11 Test Workstation (planned)
- Additional On-Premises Systems

through encrypted WireGuard tunnels.

---

# Cloudflare Integration

Cloudflare provides

- Public DNS
- HTTPS
- TLS Certificates
- Reverse Proxy
- DDoS Protection
- Web Application Firewall

Traffic Flow

```
Internet

↓

Cloudflare

↓

Application Load Balancer

↓

Amazon ECS

↓

Amazon RDS
```

---

# Security

AWS Security Groups restrict access to

Allowed

- HTTP (80)
- HTTPS (443)
- WireGuard UDP (60031)
- SSH (22) *(Administrative Access Only)*

Restricted

- Database Access
- Internal Services
- Management Ports

---

# Application Flow

```
Client

↓

Cloudflare

↓

Application Load Balancer

↓

Amazon ECS

↓

Amazon RDS PostgreSQL
```

Internal traffic between AWS, Azure, and On-Premises uses the WireGuard VPN instead of the public Internet.

---

# Current Infrastructure

| Component | Status |
|-----------|--------|
| AWS VPC | Complete |
| Public Subnet | Complete |
| Private Application Subnet | Complete |
| Private Database Subnet | Complete |
| Internet Gateway | Complete |
| Application Load Balancer | Planned |
| ECS Cluster | Planned |
| Amazon RDS | Planned |
| WireGuard Gateway | Complete |
| Site-to-Site VPN | Complete |
| Cloudflare Integration | Planned |

---

# Validation Performed

The following tests have been successfully completed.

✓ WireGuard tunnel established

✓ AWS ↔ Azure connectivity

✓ AWS ↔ On-Premises connectivity

✓ Azure ↔ On-Premises routing through AWS Hub

✓ Tunnel handshakes verified

✓ ICMP communication verified

✓ Routing tables verified

✓ WireGuard peer status verified

---

# Future Enhancements

Planned improvements include

- Multi-AZ deployment
- NAT Gateway
- Auto Scaling ECS
- Application Load Balancer deployment
- HTTPS Certificates
- Route 53 (optional)
- CloudWatch Monitoring
- AWS Systems Manager
- Automated Infrastructure using Terraform