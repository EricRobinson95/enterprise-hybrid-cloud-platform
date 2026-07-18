# 11 - Security

# Overview

This document describes the security architecture implemented within the Enterprise Hybrid Cloud Platform. Security is enforced through multiple layers including cloud-native security controls, Linux hardening, SSH authentication, and encrypted site-to-site communication using WireGuard.

The objective is to secure both cloud environments while allowing trusted communication between AWS and Azure.

---

# Security Objectives

- Protect cloud infrastructure from unauthorized access.
- Encrypt all traffic between AWS and Azure.
- Secure administrative access.
- Protect VPN authentication credentials.
- Minimize exposed services.
- Follow enterprise security best practices.

---

# Security Layers

The environment uses a layered security model.

```
Users
   │
SSH Keys
   │
Cloud Firewalls
(AWS Security Groups / Azure NSGs)
   │
Ubuntu Linux
   │
WireGuard VPN
   │
Private Cloud Networks
```

Each layer provides additional protection should another control fail.

---

# AWS Security

## Security Groups

The AWS EC2 instance is protected by a Security Group.

### Inbound Rules

| Protocol | Port | Purpose |
|----------|------|---------|
| TCP | 22 | SSH Administration |
| UDP | 51820 | WireGuard VPN |

### Outbound Rules

All outbound traffic is allowed.

---

# Azure Security

## Network Security Group (NSG)

The Azure Virtual Machine is protected by a Network Security Group.

### Inbound Rules

| Protocol | Port | Purpose |
|----------|------|---------|
| TCP | 22 | SSH Administration |
| UDP | 51820 | WireGuard VPN |

### Outbound Rules

All outbound traffic is allowed.

---

# SSH Security

Administrative access is performed using SSH public key authentication.

Password authentication is disabled.

Benefits include:

- Strong authentication
- Protection against brute-force attacks
- Elimination of password reuse
- Secure remote administration

Example connection:

```bash
ssh -i private-key.pem azureadmin@<Public-IP>
```

---

# WireGuard Security

WireGuard provides encrypted communication between AWS and Azure.

Features include:

- Public/Private Key Authentication
- ChaCha20 Encryption
- Poly1305 Authentication
- Curve25519 Key Exchange
- Perfect Forward Secrecy
- Lightweight VPN protocol

All communication between cloud providers is encrypted before traversing the public Internet.

---

# VPN Authentication

Each VPN gateway generates its own key pair.

```
AWS

Private Key
Public Key

↓

Azure

Private Key
Public Key
```

Only public keys are exchanged.

Private keys remain stored only on their respective Linux virtual machines.

---

# Private Network Isolation

AWS

```
10.0.0.0/16
```

Azure

```
10.1.0.0/16
```

These private address spaces remain isolated from the Internet.

Communication between them occurs only through the authenticated WireGuard VPN tunnel.

---

# Static Public Endpoints

Both VPN gateways use permanent public IP addresses.

AWS

- Elastic IP

Azure

- Standard Static Public IP

Benefits include:

- Stable VPN endpoints
- Consistent peer configuration
- Simplified management
- Reliable connectivity

---

# IP Forwarding

Linux IP forwarding is enabled on both VPN gateways.

```conf
net.ipv4.ip_forward=1
```

This allows the gateways to securely route traffic between AWS and Azure.

---

# Routing Security

Only trusted networks are routed through the VPN.

AWS

```
AllowedIPs = 10.1.0.0/16
```

Azure

```
AllowedIPs = 10.0.0.0/16
```

The `AllowedIPs` setting limits which remote networks are reachable through each VPN peer.

---

# No Network Address Translation (NAT)

The VPN operates using routed networking instead of NAT.

Advantages include:

- Preserves original source IP addresses
- Easier troubleshooting
- Better audit logging
- Simpler firewall policy management
- Enterprise site-to-site VPN design

Because the AWS and Azure private networks use different CIDR blocks, address translation is unnecessary.

---

# Secrets Management

Sensitive information is **never** committed to GitHub.

Protected information includes:

- WireGuard private keys
- SSH private keys
- Public IP addresses (optional)
- Cloud credentials
- Authentication tokens

Repository configuration files use placeholders instead.

Example:

```conf
PrivateKey = <AWS Private Key>
```

---

# Repository Security

The repository excludes sensitive files through `.gitignore`.

Examples include:

```
*.pem
privatekey
publickey
.env
```

Configuration files included in the repository have all sensitive values removed or replaced with placeholders.

---

# Verification

Security validation included:

- Successful SSH authentication
- Successful WireGuard peer authentication
- Encrypted VPN tunnel establishment
- Bidirectional private network communication
- Verification of cloud firewall rules
- Verification of secure routing

Evidence is available in:

```
images/testing/verification/
```

---

# Security Best Practices

This project follows several enterprise security practices.

- Principle of Least Privilege
- Key-based authentication
- Encryption in transit
- Private network segmentation
- Cloud-native firewall controls
- Dedicated VPN gateways
- Static VPN endpoints
- No hardcoded secrets
- Version-controlled infrastructure documentation

---

# Skills Demonstrated

- Cloud Security
- AWS Security Groups
- Azure Network Security Groups
- Linux Hardening
- SSH Security
- WireGuard VPN
- Public Key Cryptography
- Secure Routing
- Hybrid Cloud Security
- Infrastructure Security

---

# Future Enhancements

Potential improvements include:

- Multi-factor authentication (MFA)
- Azure Key Vault
- AWS Secrets Manager
- Cloudflare Zero Trust
- Certificate-based authentication
- Failover VPN gateways
- Intrusion detection
- Centralized logging
- Security monitoring
- SIEM integration

---

# Outcome

A layered security architecture was successfully implemented across AWS and Azure. Cloud-native firewalls, SSH key authentication, Linux security controls, and an encrypted WireGuard site-to-site VPN protect the hybrid cloud environment while enabling secure communication between private resources in both cloud providers.