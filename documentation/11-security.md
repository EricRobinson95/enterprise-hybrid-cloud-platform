# 11 - Security

# Overview

Security is a foundational component of the Enterprise Hybrid Cloud Platform. The environment is designed using a defense-in-depth strategy that combines secure networking, identity management, cloud security controls, operating system hardening, and application security.

The project separates production workloads, corporate infrastructure, and security operations into dedicated environments while maintaining encrypted communication through a WireGuard site-to-site VPN.

---

# Security Objectives

- Protect production workloads
- Secure communications between environments
- Implement enterprise identity management
- Enforce least privilege access
- Secure cloud infrastructure
- Provide a dedicated security testing environment
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
                Security Groups
                        │
                WireGuard VPN Hub
                  /             \
                 /               \
                ▼                 ▼

Azure Corporate IT      On-Premises Security Lab
      AD DS                  Kali Linux
      DNS                    Burp Suite
      Group Policy           OWASP ZAP
```

---

# Security Layers

## Layer 1 – Network Security

Traffic between environments is protected using:

- WireGuard Site-to-Site VPN
- Encrypted UDP Tunnels
- Static Routing
- Linux IP Forwarding
- Private Address Space

All inter-site communication is encrypted before traversing the public Internet.

---

## Layer 2 – Cloud Security

### AWS

Security controls include:

- Security Groups
- AWS IAM
- Private Subnets
- Least Privilege Access
- CloudWatch Logging

---

### Azure

Security controls include:

- Network Security Groups (NSGs)
- Active Directory
- Group Policy
- Windows Firewall
- Role-Based Administration

---

# Identity Security

Enterprise identity is managed using Windows Server 2025 Active Directory.

Responsibilities include:

- User Authentication
- Computer Authentication
- Group Policy
- DNS
- Security Groups
- Authorization

Example accounts

```
Eric.Admin

John.Developer
```

Administrative accounts are separated from standard user accounts to reduce risk.

---

# AWS IAM

AWS Identity and Access Management controls access to AWS resources.

IAM is responsible for:

- Administrator permissions
- Service roles
- Resource authorization
- Least privilege access

IAM does **not** replace Active Directory.

Active Directory authenticates enterprise users, while AWS IAM authorizes access to AWS resources.

---

# WireGuard Security

WireGuard provides encrypted communication between:

- AWS
- Azure
- On-Premises

Security features include:

- Public Key Cryptography
- Mutual Authentication
- Modern Encryption
- Low Attack Surface
- Minimal Configuration

Tunnel Network

```
172.16.100.0/24
```

---

# Network Segmentation

Each environment is isolated using dedicated private networks.

| Environment | Network |
|------------|----------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |

Within each environment, resources are separated into dedicated subnets.

---

# Production Security

AWS hosts the customer-facing application.

Planned production components:

- Application Load Balancer
- React Frontend
- FastAPI Backend
- PostgreSQL Database

Production security includes:

- HTTPS
- Security Groups
- Private Database Subnet
- IAM Roles
- CloudWatch Monitoring

---

# Operating System Security

## Ubuntu

Security measures include:

- SSH Key Authentication (recommended)
- UFW / iptables
- Automatic Security Updates
- WireGuard
- Least Privilege

---

## Windows Server

Security measures include:

- Active Directory
- Group Policy
- Windows Firewall
- Security Groups
- DNS Security

---

## Windows 11

Developer workstation security includes:

- Domain Authentication
- Group Policy
- Windows Defender
- Least Privilege User Account
- Remote Desktop

---

# Remote Administration

Administrative access is limited to authorized personnel.

Administration methods include:

AWS

- SSH

Azure

- Remote Desktop
- Windows Administration
- SSH (Ubuntu Gateway)

On-Premises

- SSH
- VMware Console

---

# Security Testing Environment

The on-premises environment functions as an isolated security operations lab.

Planned tools include:

- Kali Linux
- Burp Suite Community
- OWASP ZAP
- Nmap
- Wireshark
- Custom Python Security Scripts

Security testing is performed only against systems owned and managed within this project.

---

# Planned Security Testing

The production application will be evaluated using:

## Network Testing

- Host Discovery
- Port Scanning
- Service Enumeration

---

## Web Application Testing

- Authentication Testing
- Session Management
- HTTP Header Validation
- Directory Enumeration
- API Testing

---

## Vulnerability Assessment

- OWASP Top 10
- SQL Injection Testing
- Cross-Site Scripting (XSS)
- Broken Authentication
- Security Misconfiguration

Testing will be conducted only against the project environment.

---

# Logging & Monitoring

## AWS

Monitoring

- CloudWatch Metrics
- CloudWatch Logs
- Alerts

---

## Azure

Monitoring

- Azure Monitor
- Windows Event Logs

---

## Linux

Monitoring

- System Logs
- WireGuard Logs
- SSH Logs

---

# Security Validation

The following controls have been validated.

## Networking

- WireGuard VPN
- Linux Routing
- IP Forwarding
- Encrypted Tunnel Connectivity

Status

✅ Completed

---

## Cloud

Validated

- AWS Security Groups
- Azure Network Security Groups
- Private Networking

Status

✅ Completed

---

## Planned Validation

- Active Directory Security
- Group Policy
- Windows File Permissions
- Application Authentication
- HTTPS
- API Authorization
- Security Testing
- Vulnerability Assessment

---

# Current Security Posture

## Operational

- WireGuard VPN
- Secure Site-to-Site Connectivity
- AWS Security Groups
- Azure NSGs
- Linux Firewalls
- Network Segmentation

---

## In Progress

- Active Directory Deployment
- Enterprise Identity
- Developer Workstation

---

## Planned

- Application Authentication
- HTTPS
- Security Testing
- Cloud Monitoring
- Production Hardening

---

# Future Enhancements

Planned improvements include:

- Microsoft Entra ID
- AWS IAM Identity Center
- Azure Key Vault
- AWS Secrets Manager
- Multi-Factor Authentication (MFA)
- Docker Security
- Kubernetes Security
- SIEM Integration
- Prometheus
- Grafana
- Web Application Firewall (AWS WAF)
- Centralized Logging
- Automated Vulnerability Scanning

---

# Security Principles

This project follows several core security principles:

- Defense in Depth
- Least Privilege
- Network Segmentation
- Secure by Default
- Encrypted Communications
- Centralized Identity Management
- Separation of Duties

---

# Summary

Security is integrated throughout every layer of the Enterprise Hybrid Cloud Platform.

AWS hosts the production application and applies cloud-native security controls. Azure provides centralized enterprise identity and access management through Active Directory. The on-premises security lab provides a dedicated environment for validating the production application using industry-standard security tools. Together, these environments demonstrate a layered, enterprise-focused security architecture that supports secure networking, application deployment, identity management, and cybersecurity operations.