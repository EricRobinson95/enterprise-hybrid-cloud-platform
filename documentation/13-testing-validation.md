# Testing & Validation

## Overview

This document summarizes the testing and validation performed to verify the functionality of the Enterprise Hybrid Cloud Platform.

Testing was performed after each major deployment milestone to confirm that network connectivity, VPN routing, Active Directory services, and cloud infrastructure were operating as expected.

---

# Test Environment

| Environment | Network |
|------------|------------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# Validation Objectives

The following objectives were tested.

- WireGuard VPN connectivity
- Site-to-site communication
- Hybrid cloud routing
- Linux routing
- Packet forwarding
- Active Directory deployment
- DNS services
- Secure remote administration
- End-to-end connectivity

---

# AWS Validation

## EC2 Deployment

### Verified

- EC2 instance deployed
- Public IP assigned
- Private IP assigned
- Security Group configured
- Source/Destination Check disabled

Evidence

- AWS Console
- EC2 Instance Overview

Status

✅ Passed

---

## WireGuard Gateway

Verified

- WireGuard installed
- VPN interface created
- Tunnel established
- Peer communication successful

Verification Commands

```bash
wg

ip addr

ip route
```

Status

✅ Passed

---

## Linux Routing

Verified

```bash
ip route

cat /proc/sys/net/ipv4/ip_forward
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

## Azure Virtual Machine

Verified

- Ubuntu Server deployed
- Public IP assigned
- Private IP assigned
- SSH access successful

Status

✅ Passed

---

## WireGuard Gateway

Verified

```bash
wg

ip route

ip addr
```

Status

✅ Passed

---

## Windows Server 2025

Verified

- Windows Server installed
- Active Directory Domain Services installed
- DNS installed

Status

✅ Passed

---

## Active Directory

Verified

- Domain created
- Organizational Units created
- User created
- Security Group created
- User assigned to Security Group

Status

✅ Passed

---

# On-Premises Validation

## VMware

Verified

- Ubuntu Server deployed
- Network configured
- SSH configured

Status

✅ Passed

---

## WireGuard

Verified

```bash
wg

ip route

ip addr
```

Status

✅ Passed

---

# VPN Validation

## AWS ↔ Azure

Verified

- Tunnel established
- Handshake successful
- Routing successful
- ICMP successful

Evidence

- wg output
- ping
- tcpdump

Status

✅ Passed

---

## AWS ↔ On-Premises

Verified

- Tunnel established
- Handshake successful
- Routing successful
- ICMP successful

Evidence

- wg output
- ping
- tcpdump

Status

✅ Passed

---

## Azure ↔ On-Premises

Verified

- Routing through AWS Hub
- Successful packet forwarding
- Successful ICMP communication
- Successful return traffic

Evidence

- ping
- tcpdump
- wg

Status

✅ Passed

---

# Routing Validation

Verified

```bash
ip route
```

Executed on

- AWS
- Azure
- On-Premises

Confirmed

- Local networks
- Remote networks
- WireGuard routes

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
- Bidirectional packet forwarding
- Hub routing through AWS

Status

✅ Passed

---

# SSH Validation

Verified remote administration from the Windows management workstation.

Connected to

- AWS Ubuntu Server
- Azure Ubuntu Server
- On-Premises Ubuntu Server

Status

✅ Passed

---

# Screenshots

The following screenshots are included within the project documentation.

## AWS

- EC2 Overview
- Security Groups
- Route Tables
- WireGuard Status
- ip route
- iptables
- Source/Destination Check

---

## Azure

- Virtual Machine Overview
- Virtual Network
- Network Security Groups
- User Defined Routes
- Windows Server 2025
- Active Directory
- WireGuard Status
- ip route

---

## On-Premises

- VMware Topology
- WireGuard Status
- ip route

---

## VPN

- WireGuard Handshakes
- Successful Ping Tests
- tcpdump Packet Capture
- End-to-End Connectivity

---

# Issues Encountered

## WireGuard Routing

Issue

Azure and On-Premises established successful WireGuard handshakes but could not communicate through the AWS hub.

Root Cause

The WireGuard `AllowedIPs` configuration on the spoke gateways did not include the opposite spoke's tunnel IP address.

Resolution

Updated the Azure and On-Premises WireGuard configurations to include the required tunnel addresses in `AllowedIPs`.

Result

Successful end-to-end communication between Azure and the On-Premises environment.

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
| Hub-and-Spoke Routing | ✅ Passed |
| SSH Administration | ✅ Passed |
| Active Directory | ✅ Passed |
| DNS Services | ✅ Passed |
| End-to-End Connectivity | ✅ Passed |

---

# Remaining Validation

The following validation will be completed after the remaining infrastructure is deployed.

- Azure Windows 11 domain join
- On-Premises Windows 11 domain join
- Kerberos authentication
- DNS name resolution
- Group Policy application
- Cross-site authentication
- File Server access
- Internal application testing

---

# Conclusion

The Enterprise Hybrid Cloud Platform has successfully completed infrastructure deployment and network validation.

Testing confirmed secure communication between AWS, Azure, and the on-premises environment through a WireGuard hub-and-spoke VPN.

The hybrid cloud networking foundation is complete and operational. Remaining work focuses on deploying Windows 11 clients, validating Active Directory domain joins, applying Group Policy, and completing enterprise services.