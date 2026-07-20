# 13 - Testing & Validation

# Overview

This document records the testing and validation performed throughout the Enterprise Hybrid Cloud Platform project.

Testing verifies that networking, VPN connectivity, routing, cloud infrastructure, and future enterprise services operate as designed.

---

# Testing Objectives

- Verify hybrid cloud network connectivity
- Validate WireGuard VPN tunnels
- Confirm Linux routing configuration
- Verify IP forwarding
- Validate cloud network segmentation
- Test application connectivity
- Provide repeatable verification procedures
- Document expected results

---

# Test Environment

## AWS Production

| Component | Status |
|----------|--------|
| VPC | ✅ |
| EC2 | ✅ |
| WireGuard Hub | ✅ |
| Security Groups | ✅ |

---

## Azure Corporate Environment

| Component | Status |
|----------|--------|
| Virtual Network | ✅ |
| Ubuntu WireGuard Gateway | ✅ |
| Windows Server 2025 | Planned |
| Windows 11 Developer VM | Planned |

---

## On-Premises Security Lab

| Component | Status |
|----------|--------|
| Ubuntu WireGuard Gateway | ✅ |
| Kali Linux | Planned |
| Security Test Workstation | Planned |

---

# Network Validation

## IP Address Verification

Verify interface configuration.

Command

```bash
ip addr
```

Expected Result

- Correct interface addresses
- Correct subnet masks
- Correct default gateway

Status

✅ Passed

---

## Routing Table Validation

Verify Linux routing table.

Command

```bash
ip route
```

Expected Result

- Local subnet routes
- WireGuard routes
- Default gateway

Status

✅ Passed

---

## Route Lookup

Verify route selection.

Command

```bash
ip route get <destination-ip>
```

Example

```bash
ip route get 10.1.1.4
```

Expected Result

Traffic is routed through the expected interface.

Status

✅ Passed

---

# WireGuard Validation

## Tunnel Status

Verify VPN status.

Command

```bash
sudo wg
```

Verify

- Peer status
- Latest handshake
- Transfer statistics
- Allowed IPs

Status

✅ Passed

---

## WireGuard Interface

Verify interface configuration.

Command

```bash
ip addr show wg0
```

Expected Result

- Interface active
- Tunnel IP assigned

Status

✅ Passed

---

## IP Forwarding

Verify Linux forwarding.

Command

```bash
sysctl net.ipv4.ip_forward
```

Expected Result

```
net.ipv4.ip_forward = 1
```

Status

✅ Passed

---

# Connectivity Testing

## AWS → Azure

Command

```bash
ping <Azure IP>
```

Status

✅ Passed

---

## Azure → AWS

Command

```bash
ping <AWS IP>
```

Status

✅ Passed

---

## AWS → On-Premises

Command

```bash
ping <On-Premises IP>
```

Status

✅ Passed

---

## On-Premises → AWS

Command

```bash
ping <AWS IP>
```

Status

✅ Passed

---

# Traceroute Validation

Verify packet path.

Command

```bash
traceroute <destination-ip>
```

Expected Result

Traffic follows the expected route through the WireGuard VPN.

Status

✅ Passed

---

# Packet Capture Validation

Traffic was inspected using tcpdump.

## Verify ICMP

```bash
sudo tcpdump -ni wg0 icmp
```

---

## Verify WireGuard

```bash
sudo tcpdump -ni any udp port 51820
```

---

## Verify All Traffic

```bash
sudo tcpdump -ni any
```

Expected Result

- Encrypted WireGuard traffic
- ICMP traffic
- UDP encapsulation

Status

✅ Passed

---

# Cloud Network Validation

## AWS

Verified

- VPC
- Security Groups
- Route Tables
- WireGuard Hub

Status

✅ Passed

---

## Azure

Verified

- Virtual Network
- NSG Rules
- Ubuntu WireGuard Gateway

Status

✅ Passed

---

## On-Premises

Verified

- VMware Networking
- Ubuntu WireGuard Gateway
- Local Routing

Status

✅ Passed

---

# Current VPN Validation

## Successfully Verified

| Test | Status |
|------|--------|
| AWS ↔ Azure VPN | ✅ |
| AWS ↔ On-Premises VPN | ✅ |
| WireGuard Handshakes | ✅ |
| Linux Routing | ✅ |
| IP Forwarding | ✅ |
| Static Routing | ✅ |
| Packet Capture | ✅ |

---

# Current Limitation

The hybrid cloud environment currently implements a **WireGuard hub-and-spoke topology**.

The following communication paths have been validated:

- AWS ↔ Azure
- AWS ↔ On-Premises

Direct Azure ↔ On-Premises transit routing has not been implemented in the current version of the project.

This does not impact the intended architecture because:

- Azure hosts enterprise identity and developer services.
- AWS hosts the production web application.
- On-Premises performs security testing against AWS-hosted resources.

---

# Future Testing

The following validation will be completed during future project phases.

## Azure

- Active Directory
- DNS
- Group Policy
- Domain Join
- File Server
- Windows 11 Developer Workstation

---

## AWS

- React Application
- FastAPI API
- PostgreSQL
- HTTPS
- Load Balancer
- CloudWatch
- IAM

---

## Security

The on-premises security lab will validate the production environment using:

- Burp Suite Community
- OWASP ZAP
- Nmap
- Wireshark
- Vulnerability Assessment
- Authentication Testing
- Security Header Validation
- API Testing

---

# Acceptance Criteria

The project is considered operational when the following requirements are met.

## Networking

- ✅ Hybrid network deployed
- ✅ WireGuard VPN operational
- ✅ Static routing configured
- ✅ Linux routing verified

---

## Infrastructure

- Azure enterprise environment deployed
- AWS production environment deployed
- On-premises security lab operational

---

## Application

- React frontend deployed
- FastAPI backend deployed
- PostgreSQL operational
- HTTPS configured

---

## Security

- Security testing completed
- Vulnerability assessment completed
- Production validation completed

---

# Summary

Testing and validation confirm that the hybrid cloud networking foundation is operational.

AWS functions as the central WireGuard hub, providing secure connectivity to Azure and the on-premises security lab. The current implementation establishes the networking platform required for the remaining project phases, including enterprise identity services, application deployment, CI/CD, and security testing.