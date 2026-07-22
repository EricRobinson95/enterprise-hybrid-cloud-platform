# Security

## Overview

Security is a foundational component of the Enterprise Hybrid Cloud Platform. The environment is designed using a defense-in-depth strategy that combines secure networking, identity management, cloud security controls, operating system hardening, and application security.

The platform securely connects Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware environment through an encrypted WireGuard hub-and-spoke VPN while maintaining logical separation between production workloads, enterprise infrastructure, and the security testing environment.

---

# Security Objectives

The security architecture was designed to achieve the following objectives:

- Protect production workloads
- Secure communication between environments
- Centralize enterprise identity management
- Enforce least privilege access
- Secure cloud infrastructure
- Isolate security testing activities
- Demonstrate enterprise security best practices

---

# Security Architecture

```
                           Internet
                               │
                         Cloudflare DNS
                               │
                               ▼
                    AWS Production Environment
                               │
                 Application Load Balancer (Planned)
                               │
                     AWS WireGuard VPN Hub
                         172.16.100.1
                    ┌──────────┴──────────┐
                    │                     │
                    ▼                     ▼
          Azure Enterprise        On-Premises Lab
          10.1.0.0/16             10.2.0.0/16
                    │                     │
      Windows Server 2025          Kali Linux
      Active Directory             Burp Suite
      DNS Services                 OWASP ZAP
      Security Groups              Wireshark
```

---

# Defense-in-Depth

The environment uses multiple security layers.

## Layer 1 – Network Security

Implemented

- WireGuard Site-to-Site VPN
- Hub-and-Spoke VPN Architecture
- Encrypted UDP Tunnels
- Private IP Addressing
- Static Routing
- Linux IP Forwarding

All communication between AWS, Azure, and the on-premises environment is encrypted before traversing the public Internet.

---

## Layer 2 – Cloud Security

### AWS

Implemented

- Security Groups
- Private Networking
- Source/Destination Check Disabled (VPN Router)
- Linux Firewall
- SSH Administration

Planned

- IAM Roles
- CloudWatch Monitoring
- AWS WAF
- Secrets Manager

---

### Azure

Implemented

- Network Security Groups (NSGs)
- Azure Route Tables (UDRs)
- Active Directory
- DNS Server
- Windows Firewall

Planned

- Azure Backup
- Azure Monitor
- Microsoft Defender for Cloud

---

## Layer 3 – Identity Security

Enterprise identity is centralized through Windows Server 2025 Active Directory hosted in Azure.

Implemented

- Active Directory Domain Services
- DNS
- Organizational Units
- Administrative User
- Administrative Security Group

Planned

- Windows 11 Domain Join
- Group Policy Objects
- Additional Security Groups
- Kerberos Validation

---

# Identity and Access Management

## Active Directory

Active Directory provides:

- User Authentication
- Computer Authentication
- Centralized Authorization
- DNS
- Security Group Management
- Enterprise Identity Services

Administrative accounts are separated from standard user accounts to reduce administrative risk.

---

## AWS IAM

AWS Identity and Access Management controls authorization within AWS.

Responsibilities include:

- Administrative permissions
- Service roles
- Resource authorization
- Least privilege access

AWS IAM complements Active Directory rather than replacing it.

---

# VPN Security

Secure communication between cloud environments is provided through WireGuard.

Current VPN Features

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

Current Tunnel Endpoints

| Gateway | Tunnel Address |
|----------|----------------|
| AWS | 172.16.100.1 |
| Azure | 172.16.100.2 |
| On-Premises | 172.16.100.3 |

---

# Network Segmentation

The enterprise environment uses dedicated private address spaces.

| Environment | Network |
|------------|----------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |

Each environment is further segmented into dedicated subnets for infrastructure, clients, applications, and management services.

---

# Operating System Security

## Ubuntu Linux

Implemented

- WireGuard VPN
- Linux IP Forwarding
- iptables Firewall
- Secure SSH Administration

Recommended

- SSH Key Authentication
- Automatic Security Updates
- Fail2Ban

---

## Windows Server 2025

Implemented

- Active Directory Domain Services
- DNS Server
- Windows Firewall
- Administrative User
- Administrative Security Group

Planned

- Group Policy
- File Server
- Security Baselines

---

## Windows 11 Enterprise

Planned

- Domain Authentication
- Group Policy
- Windows Defender
- Least Privilege User Accounts
- BitLocker
- Windows Firewall

---

# Remote Administration

Current administrative access methods include:

AWS

- SSH

Azure

- Remote Desktop (Windows Server)
- SSH (Ubuntu Gateway)

On-Premises

- SSH
- VMware Console

Administrative access is limited to authorized accounts.

---

# Security Operations Lab

The on-premises VMware environment serves as an isolated security testing lab.

Current Resources

- Kali Linux
- Ubuntu WireGuard Gateway

Planned Tools

- Burp Suite Community
- OWASP ZAP
- Nmap
- Wireshark
- Custom Python Security Scripts

Testing is performed only against infrastructure owned and managed within this project.

---

# Planned Security Testing

## Network Security

- Host Discovery
- Port Scanning
- Service Enumeration
- Firewall Validation

---

## Web Application Security

- Authentication Testing
- Session Management
- HTTP Header Validation
- API Security
- Directory Enumeration

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

The following security controls have been validated.

## Network Security

Validated

- WireGuard VPN
- Encrypted Site-to-Site Connectivity
- Linux IP Forwarding
- Packet Forwarding
- Cross-Site Routing

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

- Active Directory Installation
- DNS Installation
- Administrative User
- Security Group Creation

Status

✅ Complete

---

## Remaining Validation

- Windows 11 Domain Join
- Group Policy
- Kerberos Authentication
- DNS Resolution Across VPN
- File Server Permissions
- Application Authentication
- HTTPS
- API Authorization

---

# Current Security Posture

## Operational

- WireGuard Hub-and-Spoke VPN
- AWS Security Groups
- Azure Network Security Groups
- Azure Route Tables
- Linux Firewalls
- Active Directory
- DNS
- Administrative User
- Administrative Security Group
- Network Segmentation

---

## In Progress

- Windows 11 Enterprise Clients
- Enterprise Identity Expansion
- Group Policy

---

## Planned

- File Server
- Internal Application Server
- HTTPS
- Cloud Monitoring
- Vulnerability Assessments
- Application Security Testing

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
- Prometheus
- Grafana
- SIEM Integration
- AWS WAF
- Centralized Logging
- Automated Vulnerability Scanning
- Infrastructure as Code Security

---

# Security Principles

The Enterprise Hybrid Cloud Platform follows these core security principles:

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

# Summary

Security is integrated into every layer of the Enterprise Hybrid Cloud Platform.

AWS provides the production cloud environment and serves as the WireGuard VPN hub. Azure hosts centralized identity services through Windows Server 2025 Active Directory and DNS. The on-premises VMware environment provides an isolated security operations lab for validating enterprise infrastructure and future web applications.

The networking foundation has been fully secured through encrypted site-to-site connectivity, centralized identity management, network segmentation, cloud-native security controls, and layered defense mechanisms. Future phases will expand security through Windows 11 domain integration, Group Policy, application security testing, cloud monitoring, and advanced identity services.