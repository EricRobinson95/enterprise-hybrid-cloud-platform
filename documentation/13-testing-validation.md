# 13 - Testing & Validation

# Overview

This document summarizes the testing and validation performed throughout the implementation of the Enterprise Hybrid Cloud Platform.

Each infrastructure component was validated after deployment to verify enterprise networking, VPN connectivity, routing, Active Directory services, DNS resolution, Windows domain authentication, and secure administration.

The testing process confirmed that AWS, Microsoft Azure, and the on-premises VMware environment operate as a unified hybrid cloud infrastructure connected through an encrypted WireGuard site-to-site VPN.

---

# Test Environment

| Environment | Network |
|-------------|-------------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# Validation Objectives

The following objectives were validated.

- WireGuard VPN connectivity
- Site-to-site communication
- Enterprise routing
- Linux routing
- Azure User Defined Routes
- Windows persistent routes
- Active Directory deployment
- Enterprise DNS
- Domain authentication
- Secure remote administration
- Cross-site connectivity

---

# AWS Validation

## EC2 WireGuard Gateway

Verified

- EC2 instance deployment
- Public IP assignment
- Private IP assignment
- Security Group configuration
- Source/Destination Check disabled

Evidence

- AWS Console
- EC2 Instance Overview

Status

✅ Passed

---

## WireGuard

Verified

```bash
wg

ip addr

ip route
```

Confirmed

- Peer handshakes
- Tunnel interfaces
- Remote routes

Status

✅ Passed

---

## Linux Routing

Verified

```bash
ip route
```

Confirmed

- Local routes
- Azure routes
- On-Premises routes

Status

✅ Passed

---

## IP Forwarding

Verified

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Expected

```
1
```

Status

✅ Passed

---

## Firewall

Verified

```bash
iptables -L

iptables -t nat -L
```

Status

✅ Passed

---

# Azure Validation

## Ubuntu WireGuard Gateway

Verified

```bash
wg

ip addr

ip route
```

Confirmed

- Tunnel interfaces
- Peer connectivity
- Linux routing

Status

✅ Passed

---

## Azure User Defined Routes

Verified

- Route table created
- Gateway Subnet associated
- Identity Services Subnet associated
- Remote routes configured

Status

✅ Passed

---

## Windows Server 2025

Verified

- Server deployment
- Active Directory installation
- DNS installation

Verification

```powershell
Get-ADDomain
```

Status

✅ Passed

---

## Active Directory

Verified

- Domain creation
- Organizational Units
- Enterprise users
- Security groups
- Computer objects

Verification

```powershell
Get-ADUser

Get-ADComputer
```

Status

✅ Passed

---

# On-Premises Validation

## Ubuntu WireGuard Gateway

Verified

```bash
wg

ip addr

ip route
```

Status

✅ Passed

---

## Windows 11 Enterprise

Verified

- Domain Join
- Active Directory Login
- Enterprise DNS

Verification

```powershell
systeminfo

hostname

whoami
```

Status

✅ Passed

---

# VPN Validation

## AWS ↔ Azure

Verified

- Tunnel established
- Handshakes successful
- ICMP communication
- Route validation

Evidence

- wg
- ping
- tcpdump

Status

✅ Passed

---

## AWS ↔ On-Premises

Verified

- Tunnel established
- Handshakes successful
- ICMP communication
- Route validation

Evidence

- wg
- ping
- tcpdump

Status

✅ Passed

---

## Azure ↔ On-Premises

Verified

- Routing through AWS Hub
- Bidirectional communication
- Packet forwarding
- Return traffic

Evidence

- ping
- tracert
- tcpdump
- wg

Status

✅ Passed

---

# Routing Validation

Validated on:

- AWS
- Azure
- On-Premises Linux
- Windows 11

Verification

```bash
ip route
```

```powershell
route print
```

Confirmed

- Local routes
- Remote routes
- WireGuard routes
- Windows persistent routes

Status

✅ Passed

---

# DNS Validation

Verified

- Active Directory DNS
- Internal name resolution
- Cross-site DNS resolution

Verification

```powershell
nslookup
```

Status

✅ Passed

---

# Active Directory Validation

Verified

- Domain authentication
- Enterprise users
- Organizational Units
- Computer objects
- Domain-joined Windows workstations

Verification

```powershell
whoami

Get-ADUser

Get-ADComputer
```

Status

✅ Passed

---

# Packet Forwarding Validation

Verified

```bash
tcpdump -ni wg0
```

Confirmed

- ICMP Echo Requests
- ICMP Echo Replies
- DNS Queries
- Bidirectional forwarding
- Routing through AWS Hub

Status

✅ Passed

---

# SSH Validation

Verified secure administration from the Windows management workstation.

Connected to

- AWS Ubuntu Gateway
- Azure Ubuntu Gateway
- On-Premises Ubuntu Gateway

Status

✅ Passed

---

# Remote Desktop Validation

Verified

- Windows Server 2025 Remote Desktop
- Enterprise administration

Status

✅ Passed

---

# Screenshot Evidence

The repository includes screenshots documenting the completed deployment.

## AWS

- EC2 Overview
- Security Groups
- Route Tables
- WireGuard Status
- Linux Routing
- Firewall Rules

---

## Azure

- Virtual Network
- User Defined Routes
- Route Table Associations
- Network Security Groups
- WireGuard Gateway
- Windows Server 2025
- Active Directory
- DNS
- Organizational Units
- Users
- Groups
- Computer Objects

---

## On-Premises

- VMware Environment
- Ubuntu WireGuard Gateway
- Windows 11 Enterprise
- Kali Linux
- Domain Join

---

## Enterprise Validation

- Successful Ping Tests
- Traceroute Results
- WireGuard Handshakes
- DNS Resolution
- Domain Login
- Cross-site Authentication
- Packet Captures

---

# Issues Encountered

## Hybrid Routing

Issue

WireGuard tunnels established successfully, but Azure and the on-premises environment could not communicate.

Root Cause

Azure User Defined Routes were not associated with every required subnet.

Resolution

Associated the Azure route table with both:

- Gateway Subnet
- Identity Services Subnet

Result

Cross-site communication was immediately restored.

Status

✅ Resolved

---

## Windows Routing

Issue

Windows workstations could not reach remote enterprise networks.

Root Cause

Persistent routes were missing.

Resolution

Configured Windows persistent routes pointing to the local WireGuard gateway.

Status

✅ Resolved

---

## WireGuard AllowedIPs

Issue

VPN handshakes succeeded while forwarded traffic failed.

Root Cause

Tunnel endpoint addresses were missing from `AllowedIPs`.

Resolution

Updated the Azure and On-Premises peer configurations.

Status

✅ Resolved

---

# Current Validation Status

| Component | Status |
|-----------|--------|
| AWS Infrastructure | ✅ Passed |
| Azure Infrastructure | ✅ Passed |
| On-Premises Infrastructure | ✅ Passed |
| WireGuard VPN | ✅ Passed |
| Linux Routing | ✅ Passed |
| Azure User Defined Routes | ✅ Passed |
| Windows Persistent Routes | ✅ Passed |
| Active Directory | ✅ Passed |
| Enterprise DNS | ✅ Passed |
| Azure Windows 11 Domain Join | ✅ Passed |
| On-Premises Windows 11 Domain Join | ✅ Passed |
| Cross-site Authentication | ✅ Passed |
| Cross-site DNS Resolution | ✅ Passed |
| End-to-End Connectivity | ✅ Passed |

---

# Remaining Validation

The following validation will be completed during future project phases.

- Group Policy deployment
- Windows File Server
- Internal Application Server
- HTTPS certificates
- Web application deployment
- React frontend
- FastAPI backend
- PostgreSQL connectivity
- GitHub Actions deployment
- Burp Suite testing
- OWASP ZAP assessment
- Cloud monitoring

---

# Lessons Learned

The implementation reinforced several important enterprise networking concepts.

- VPN handshakes do not guarantee end-to-end communication.
- Azure User Defined Route associations are critical in hybrid networking.
- Windows clients require persistent routes when the VPN gateway is not the default gateway.
- Active Directory depends on functional DNS and routing.
- Packet captures using `tcpdump` greatly simplify VPN troubleshooting.
- Systematic validation after every deployment phase reduces troubleshooting time.

---

# Conclusion

Testing and validation confirmed that the Enterprise Hybrid Cloud Platform operates as a fully functional hybrid enterprise infrastructure.

The completed implementation includes encrypted WireGuard site-to-site connectivity, enterprise routing, Azure User Defined Routes, Windows persistent routing, centralized Active Directory, enterprise DNS, domain-joined Windows workstations, and validated cross-site authentication between Azure and the on-premises environment.

This validated infrastructure provides a production-style foundation for the remaining phases of the project, including Group Policy, enterprise file services, CI/CD automation, application hosting, monitoring, and web application security testing.