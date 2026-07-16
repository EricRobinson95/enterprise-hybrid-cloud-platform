# 10 - Security

# Overview

Security is a core design principle of the Enterprise Hybrid Cloud Platform. The environment follows a defense-in-depth strategy by implementing multiple layers of protection across identity management, networking, VPN connectivity, cloud infrastructure, and operating systems.

Rather than relying on a single security mechanism, the platform combines AWS security services, Azure security controls, encrypted VPN tunnels, and least-privilege access to reduce the attack surface and protect enterprise resources.

---

# Security Objectives

- Protect cloud resources from unauthorized access.
- Encrypt all communication between AWS and Azure.
- Restrict administrative access.
- Isolate application and database workloads.
- Implement least privilege throughout the environment.
- Secure Internet-facing services.
- Monitor and log security events.
- Prepare the environment for future automation and compliance.

---

# Identity and Access Management (IAM)

Administrative access to AWS is managed using AWS Identity and Access Management (IAM).

## Root Account

The AWS root account is reserved exclusively for emergency administrative tasks.

The root account is **not** used for daily administration.

Root account responsibilities include:

- Billing management
- Account recovery
- MFA management
- Emergency administrative operations

---

## IAM Administrator Account

Daily administration is performed using a dedicated IAM user.

Configuration

| Setting | Value |
|----------|-------|
| User | AdminEric |
| MFA | Enabled |
| Group | Administration |

Benefits

- Separate identity from the root account.
- Supports auditing and accountability.
- Limits exposure of the root account.
- Follows AWS security best practices.

---

## IAM Groups

Permissions are assigned through IAM groups rather than directly to users.

Current Groups

- Administration
- Cloud_Engineers
- Developers
- Network_Engineers

This approach simplifies future user management and follows enterprise identity management practices.

---

# Multi-Factor Authentication (MFA)

Multi-Factor Authentication is enabled for:

- Root Account
- IAM Administrator Account

Benefits

- Protects against stolen passwords.
- Requires possession of a trusted authentication device.
- Significantly reduces the risk of unauthorized access.

---

# Principle of Least Privilege

The environment follows the Principle of Least Privilege.

Resources receive only the permissions required to perform their intended functions.

Examples

- SSH access restricted to administrator public IP.
- Private resources are not Internet accessible.
- Security groups allow only required ports.
- IAM users receive permissions through groups.

---

# Virtual Private Cloud (VPC) Security

AWS networking provides isolation between Internet-facing resources and private workloads.

Network Layout

```
Internet
    │
Internet Gateway
    │
Public Subnet
    │
WireGuard Gateway
    │
Encrypted VPN
    │
Azure
```

Private resources remain isolated from direct Internet access.

---

# Network Segmentation

The AWS environment is segmented into dedicated network zones.

## Public Subnet

CIDR

```
10.0.1.0/24
```

Purpose

- WireGuard VPN Gateway
- Future Application Load Balancer

Internet Accessible

Yes

---

## Private Application Subnet

CIDR

```
10.0.2.0/24
```

Purpose

- Application Servers
- Containers

Internet Accessible

No

---

## Private Database Subnet

CIDR

```
10.0.3.0/24
```

Purpose

- Amazon RDS
- Internal Database Services

Internet Accessible

No

---

# Security Groups

Security Groups provide stateful firewall protection for EC2 instances.

Current Security Group

```
production-wireguard-sg
```

---

## Inbound Rules

| Protocol | Port | Source | Purpose |
|----------|------|--------|---------|
| TCP | 22 | Administrator Public IP (/32) | SSH Administration |
| UDP | 51820 | 0.0.0.0/0 | WireGuard VPN Clients |

SSH is restricted to the administrator's current public IP, significantly reducing the attack surface while allowing secure remote management.

WireGuard listens on UDP port 51820 to allow encrypted VPN communication from Azure.

---

## Outbound Rules

| Protocol | Destination | Purpose |
|----------|-------------|---------|
| All Traffic | 0.0.0.0/0 | Outbound Communication |

Outbound traffic is currently unrestricted to allow:

- Ubuntu updates
- Package installation
- WireGuard communication
- Azure connectivity

Outbound rules may become more restrictive as the environment matures.

---

# Security Group Behavior

AWS Security Groups are **stateful** firewalls.

This means:

- Allowed inbound traffic automatically allows return traffic.
- Allowed outbound traffic automatically allows return traffic.

No additional rules are required for response traffic.

---

# Network ACLs

Network ACLs provide subnet-level protection.

Characteristics

- Stateless firewall
- Applied to subnets
- Processes inbound and outbound traffic independently

Current Status

The environment currently uses the default Network ACL.

Future versions of the project may implement custom Network ACLs for additional subnet protection.

---

# WireGuard VPN Security

Communication between AWS and Azure is protected using WireGuard.

Benefits

- Modern cryptography
- High performance
- Lightweight implementation
- Mutual authentication
- End-to-end encryption

WireGuard protects all traffic exchanged between the AWS VPC and Azure Virtual Network.

---

# SSH Security

Administrative access uses SSH public key authentication.

Authentication Method

- RSA Key Pair
- Private Key (.pem)
- Public Key installed on EC2

Benefits

- Strong cryptographic authentication.
- Eliminates password-based logins.
- Protects against brute-force password attacks.

The private SSH key is stored securely by the administrator and is never committed to source control.

---

# Operating System Security

Operating System

```
Ubuntu Server 26.04 LTS
```

Security Benefits

- Long-Term Support (LTS)
- Regular security updates
- Stable enterprise platform
- Large security community

---

# Future Operating System Hardening

Future improvements include:

- Disable password authentication.
- Disable root SSH login.
- Configure UFW firewall.
- Enable automatic security updates.
- Configure Fail2Ban.
- Install intrusion detection tools.

---

# Cloud Security Best Practices

Current Implementation

- Dedicated IAM Administrator Account
- Root account reserved for emergencies
- Multi-Factor Authentication enabled
- Dedicated VPC
- Public and private subnet separation
- Least Privilege Security Groups
- SSH restricted to administrator public IP
- WireGuard encrypted VPN
- Enterprise naming conventions

---

# Future Security Enhancements

Planned improvements include:

- AWS Systems Manager (Session Manager)
- AWS CloudTrail
- AWS GuardDuty
- AWS Config
- AWS Security Hub
- AWS Backup
- CloudWatch security monitoring
- AWS WAF
- Elastic IP for WireGuard gateway
- NAT Gateway for private subnet Internet access
- Custom Network ACLs
- Bastion Host (optional)
- Infrastructure as Code using Terraform

---

# Security Layers

```
Administrator
        │
MFA
        │
IAM
        │
Security Group
        │
WireGuard Encryption
        │
VPC Segmentation
        │
Private Subnets
        │
Application Layer
```

Each layer provides additional protection, creating a defense-in-depth architecture that minimizes the impact of individual security failures.

---

# Security Summary

Current Security Features

- Dedicated AWS VPC
- Public and private subnet isolation
- Internet Gateway with controlled routing
- Dedicated Security Group
- SSH restricted to administrator public IP
- WireGuard VPN encryption
- IAM Administrator account
- Root account reserved for emergencies
- Multi-Factor Authentication enabled
- Ubuntu Server LTS
- SSH key authentication
- Principle of Least Privilege
- Enterprise network segmentation

The platform is designed to follow modern enterprise cloud security practices while remaining scalable for future expansion into a complete hybrid cloud environment.