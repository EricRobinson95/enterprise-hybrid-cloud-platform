# 12 - Security

# Overview

Security is a foundational component of the Enterprise Hybrid Cloud Platform. The environment implements a defense-in-depth strategy by combining secure networking, centralized identity management, cloud-native security controls, operating system hardening, and network segmentation.

Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware enterprise environment are securely connected using an encrypted WireGuard hub-and-spoke VPN. Enterprise identity is centralized through Active Directory Domain Services (AD DS), allowing secure authentication across cloud and on-premises resources.

The architecture separates production infrastructure, enterprise identity services, and security operations into dedicated environments while enforcing layered security controls.

---

# Security Objectives

The security architecture was designed to provide:

- Secure hybrid cloud networking
- Centralized identity management
- Least privilege administration
- Secure remote administration
- Enterprise network segmentation
- Encrypted communications
- Defense in depth
- Secure authentication across multiple sites
- Separation of production and security testing environments

---

# Enterprise Security Architecture

```
                           Internet
                               │
                         Cloudflare DNS
                               │
                        AWS Production
                               │
                    WireGuard VPN Hub
                      172.16.100.1
               ┌─────────────┴─────────────┐
               │                           │
               ▼                           ▼

      Azure Enterprise            On-Premises Enterprise
        10.1.0.0/16                  10.2.0.0/16

 Windows Server 2025           Windows 11 Enterprise
 Active Directory              Kali Linux
 Enterprise DNS                Security Testing
```

---

# Defense-in-Depth

The platform uses multiple layers of security.

## Layer 1 – Network Security

Implemented

- WireGuard Site-to-Site VPN
- Hub-and-Spoke Architecture
- Encrypted VPN Tunnels
- Private Address Space
- Linux Routing
- Linux IP Forwarding
- Azure User Defined Routes
- Windows Persistent Routes

Enterprise traffic is encrypted before traversing the public Internet.

---

## Layer 2 – Cloud Security

### Amazon Web Services

Implemented

- Security Groups
- VPC Isolation
- Internet Gateway
- Linux Firewall
- Source/Destination Check Disabled
- Secure SSH Administration

Planned

- CloudWatch
- IAM Roles
- AWS WAF
- Secrets Manager

---

### Microsoft Azure

Implemented

- Network Security Groups
- User Defined Routes
- Route Table Associations
- Windows Firewall
- Active Directory
- Enterprise DNS

Planned

- Azure Backup
- Azure Monitor
- Microsoft Defender for Cloud

---

## Layer 3 – Identity Security

Enterprise identity is centralized through Windows Server 2025 Active Directory.

Implemented

- Active Directory Domain Services
- Enterprise DNS
- Organizational Units
- Enterprise Users
- Security Groups
- Domain-Joined Windows Workstations
- Cross-site Authentication

Planned

- Group Policy
- Fine-Grained Password Policies

---

# Identity and Access Management

## Active Directory

Active Directory provides:

- User Authentication
- Computer Authentication
- Centralized Authorization
- Enterprise DNS
- Organizational Unit Management
- Security Group Management

Administrative accounts are separated from standard user accounts in accordance with least privilege principles.

---

## AWS Identity and Access Management

AWS IAM controls access to AWS resources.

Responsibilities include:

- Administrative Permissions
- Service Roles
- Resource Authorization
- Least Privilege Access

AWS IAM manages cloud resources while Active Directory manages enterprise user identities.

---

# VPN Security

WireGuard secures communication between every enterprise location.

Implemented Features

- Site-to-Site VPN
- Hub-and-Spoke Architecture
- Public Key Authentication
- Mutual Authentication
- ChaCha20 Encryption
- Poly1305 Authentication
- Curve25519 Key Exchange
- Perfect Forward Secrecy

Tunnel Network

```
172.16.100.0/24
```

Tunnel Endpoints

| Gateway | Tunnel Address |
|----------|----------------|
| AWS Hub | 172.16.100.1 |
| Azure Gateway | 172.16.100.2 |
| On-Premises Gateway | 172.16.100.3 |

---

# Network Segmentation

Dedicated private address spaces isolate each enterprise environment.

| Environment | Network |
|-------------|-------------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |

Each environment is further segmented into dedicated subnets for infrastructure, enterprise clients, identity services, and future application hosting.

---

# Operating System Security

## Ubuntu Linux

Implemented

- WireGuard
- Linux Routing
- IP Forwarding
- iptables Firewall
- Secure SSH Administration

Future Improvements

- SSH Key Authentication
- Automatic Security Updates
- Fail2Ban

---

## Windows Server 2025

Implemented

- Active Directory Domain Services
- DNS Server
- Windows Firewall
- Enterprise User Management
- Organizational Units
- Security Groups

Future Improvements

- Group Policy
- File Services
- Security Baselines

---

## Windows 11 Enterprise

Implemented

- Domain Authentication
- Enterprise DNS
- Windows Firewall
- Active Directory Authentication

Planned

- Group Policy
- BitLocker
- Windows Defender Hardening
- Application Control

---

# Remote Administration

Current administrative methods

AWS

- SSH

Azure

- Remote Desktop
- SSH

On-Premises

- SSH
- VMware Console

Windows 11 developer workstations do not receive Remote Desktop privileges, following enterprise least privilege practices.

---

# Security Operations Lab

The on-premises VMware environment functions as an isolated security operations lab.

Current Resources

- Kali Linux
- Ubuntu WireGuard Gateway
- Windows 11 Enterprise Workstation

Planned Tools

- Burp Suite Community
- OWASP ZAP
- Wireshark
- Nmap
- Custom Python Security Tools

Security testing is performed exclusively against infrastructure owned by this project.

---

# Planned Security Testing

## Network Security

- Host Discovery
- Port Scanning
- Firewall Validation
- Service Enumeration

---

## Web Application Security

- Authentication Testing
- Session Management
- HTTPS Validation
- API Security
- HTTP Header Validation

---

## Vulnerability Assessment

- OWASP Top 10
- SQL Injection
- Cross-Site Scripting (XSS)
- Security Misconfiguration
- Broken Authentication

---

# Logging and Monitoring

## AWS

Planned

- CloudWatch Metrics
- CloudWatch Logs
- CloudWatch Alarms

---

## Azure

Current

- Windows Event Logs

Planned

- Azure Monitor
- Microsoft Defender for Cloud

---

## Linux

Current

- System Logs
- SSH Logs
- WireGuard Logs

---

# Security Validation

## Network Security

Validated

- WireGuard VPN
- Linux Routing
- Linux IP Forwarding
- Azure User Defined Routes
- Windows Persistent Routes
- Cross-site Routing

Status

✅ Complete

---

## Cloud Security

Validated

- AWS Security Groups
- Azure Network Security Groups
- Azure Route Tables
- Network Segmentation

Status

✅ Complete

---

## Identity Security

Validated

- Active Directory
- Enterprise DNS
- Organizational Units
- Security Groups
- Enterprise Users
- Domain-Joined Windows 11 Workstations
- Cross-site Authentication

Status

✅ Complete

---

## DNS

Validated

- Cross-site DNS Resolution

Status

✅ Complete

---

# Current Security Posture

## Operational

- WireGuard Site-to-Site VPN
- Hub-and-Spoke VPN Architecture
- AWS Security Groups
- Azure Network Security Groups
- Azure User Defined Routes
- Windows Firewall
- Linux Firewall
- Active Directory
- Enterprise DNS
- Organizational Units
- Security Groups
- Domain-Joined Windows Workstations
- Cross-site Authentication
- Network Segmentation

---

## Planned

- Group Policy
- Windows File Server
- Internal Application Server
- HTTPS
- Cloud Monitoring
- Vulnerability Assessments
- Web Application Security Testing

---

# Future Enhancements

Planned improvements include:

- Microsoft Entra ID
- Microsoft Entra Connect
- Azure Key Vault
- AWS Secrets Manager
- AWS IAM Identity Center
- Multi-Factor Authentication (MFA)
- Docker Security
- Kubernetes Security
- AWS WAF
- Prometheus
- Grafana
- SIEM Integration
- Centralized Logging
- Automated Vulnerability Scanning
- Infrastructure as Code Security

---

# Security Principles

The Enterprise Hybrid Cloud Platform follows these security principles:

- Defense in Depth
- Least Privilege
- Zero Trust Mindset
- Network Segmentation
- Secure by Default
- Encrypted Communications
- Centralized Identity Management
- Separation of Duties
- Layered Security Controls

---

# Lessons Learned

Implementing the security architecture reinforced several important concepts:

- Identity services rely on secure networking and DNS.
- VPN encryption alone does not guarantee connectivity; routing must also be correctly configured.
- Azure User Defined Routes are essential for hybrid cloud networking.
- Domain-joined workstations simplify centralized administration.
- Separating production infrastructure, identity services, and security operations reduces operational risk.
- Layered security controls provide significantly stronger protection than relying on a single security mechanism.

---

# Summary

Security is integrated into every layer of the Enterprise Hybrid Cloud Platform.

AWS provides secure production networking and serves as the WireGuard VPN hub. Microsoft Azure hosts centralized identity services through Windows Server 2025 Active Directory and enterprise DNS. The on-premises VMware environment provides enterprise workstations and an isolated security operations lab for validating infrastructure and future applications.

The completed implementation includes encrypted site-to-site connectivity, centralized identity management, domain-joined Windows workstations, enterprise DNS, network segmentation, cloud-native security controls, and validated cross-site authentication. Future project phases will build on this foundation with Group Policy, application hosting, cloud monitoring, advanced identity services, and web application security testing.