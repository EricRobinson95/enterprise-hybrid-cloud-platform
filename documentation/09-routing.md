# 09 - Routing

# Overview

Routing controls how traffic moves between resources within the AWS Virtual Private Cloud (VPC), the public Internet, and the future Azure Virtual Network.

The Enterprise Hybrid Cloud Platform uses dedicated route tables to control traffic flow, enforce network segmentation, and securely connect AWS and Azure through a WireGuard site-to-site VPN.

---

# Routing Objectives

- Enable communication between AWS resources.
- Provide Internet access only where required.
- Isolate application and database workloads.
- Prepare for secure hybrid cloud connectivity.
- Follow enterprise networking best practices.

---

# AWS Routing Components

The AWS environment currently consists of:

- Production VPC
- Public Route Table
- Internet Gateway
- Public Subnet
- Private Application Subnet
- Private Database Subnet

These components work together to determine how packets travel through the network.

---

# VPC Local Routing

Every AWS VPC automatically creates a local route.

Current Route

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | Local |

Purpose

The local route allows every subnet inside the VPC to communicate with each other.

Example

```
EC2 (10.0.1.x)
        │
        ▼
Application (10.0.2.x)
        │
        ▼
Database (10.0.3.x)
```

Traffic between these resources never leaves the AWS network.

---

# Internet Routing

Internet access is provided through an Internet Gateway.

Current Route

| Destination | Target |
|-------------|--------|
| 0.0.0.0/0 | Internet Gateway |

Purpose

This route sends all traffic destined for networks outside the VPC to the Internet Gateway.

Without this route, resources cannot communicate with the public Internet.

---

# Public Route Table

Name

```
production-public-route-table
```

Routes

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | Internet Gateway |

Associated Subnet

- public-subnet

Purpose

Allows Internet-facing resources to communicate with external networks while maintaining local communication within the VPC.

---

# Main Route Table

The default route table currently contains only the automatically created local route.

Routes

| Destination | Target |
|-------------|--------|
| 10.0.0.0/16 | Local |

Associated Subnets

- private-application-subnet
- private-database-subnet

Purpose

Provides communication within the VPC while preventing direct Internet access.

---

# Route Table Associations

Current Associations

```
production-public-route-table
        │
        ▼
public-subnet
```

```
Main Route Table
        │
        ├── private-application-subnet
        └── private-database-subnet
```

Only the public subnet has Internet access.

---

# Packet Flow

## Internet Client to AWS

```
Client
    │
Internet
    │
Internet Gateway
    │
Public Route Table
    │
Public Subnet
    │
Application Load Balancer (Future)
```

---

## Administrator SSH Access

```
Administrator Laptop
        │
SSH
        │
Internet
        │
Internet Gateway
        │
Public Route Table
        │
Public Subnet
        │
production-wireguard-gateway
```

---

## Internal AWS Communication

```
WireGuard Gateway
        │
10.0.0.0/16 (Local Route)
        │
Private Application Subnet
        │
Private Database Subnet
```

No Internet Gateway is required because communication stays inside the VPC.

---

# Public vs Private Routing

## Public Subnet

```
10.0.1.0/24
```

Characteristics

- Internet Gateway route
- Public IP support
- Internet accessible
- Hosts public-facing resources

---

## Private Application Subnet

```
10.0.2.0/24
```

Characteristics

- Local routing only
- No Internet Gateway route
- Not directly Internet accessible
- Hosts application services

---

## Private Database Subnet

```
10.0.3.0/24
```

Characteristics

- Local routing only
- No Internet Gateway route
- Isolated from the Internet
- Hosts database services

---

# Future Hybrid Cloud Routing

The completed project will introduce additional routes through the WireGuard VPN gateway.

Future Traffic Flow

```
AWS Application
        │
Private Route Table
        │
WireGuard Gateway
        │
Encrypted VPN Tunnel
        │
Azure Virtual Network
        │
Windows Services
```

Traffic destined for Azure will be forwarded through the WireGuard VPN instead of the public Internet.

---

# Future Route Tables

As the environment grows, additional dedicated route tables will be created.

Planned Route Tables

- production-public-route-table
- production-private-app-route-table
- production-private-db-route-table

Benefits

- Improved network segmentation
- Easier administration
- Better scalability
- More granular routing control

---

# Future NAT Gateway

Private application resources will eventually require outbound Internet access for:

- Ubuntu package updates
- Docker image downloads
- Operating system updates

This will be provided through a NAT Gateway.

Future Flow

```
Private Application
        │
Private Route Table
        │
NAT Gateway
        │
Internet Gateway
        │
Internet
```

The NAT Gateway allows outbound communication without exposing private resources to inbound Internet traffic.

---

# Routing Design Decisions

- Dedicated public route table for Internet-facing resources.
- Private resources remain isolated from direct Internet access.
- Internet Gateway attached only to the production VPC.
- Public subnet receives Internet access through route table association.
- Local VPC routing enables internal communication between all AWS subnets.
- Future Azure traffic will traverse the WireGuard VPN tunnel.
- Future NAT Gateway will provide secure outbound Internet access for private workloads.

---

# Current Routing Status

| Component | Status |
|-----------|--------|
| VPC Local Route | ✅ Complete |
| Internet Gateway | ✅ Complete |
| Public Route Table | ✅ Complete |
| Public Subnet Association | ✅ Complete |
| Internet Routing | ✅ Complete |
| Private Route Tables | ⏳ Planned |
| NAT Gateway | ⏳ Planned |
| WireGuard Routes | ⏳ Planned |
| Azure Routes | ⏳ Planned |

---

# Summary

The AWS routing infrastructure has been configured to support a secure enterprise network architecture.

Current routing provides:

- Internal communication between AWS resources.
- Controlled Internet access for public resources.
- Network segmentation through dedicated subnets.
- A foundation for future WireGuard VPN connectivity with Azure.
- A scalable routing design that supports future application and database deployments.