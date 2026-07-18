# 10 - Routing

# Overview

This document describes the routing architecture used within the Enterprise Hybrid Cloud Platform. Routing enables secure communication between the Amazon Web Services (AWS) Virtual Private Cloud (VPC) and the Microsoft Azure Virtual Network (VNet) through an encrypted WireGuard site-to-site VPN.

The routing design allows private resources in each cloud provider to communicate directly while maintaining independent network boundaries and avoiding Network Address Translation (NAT).

---

# Objectives

- Enable communication between AWS and Azure private networks.
- Route private traffic through the WireGuard VPN tunnel.
- Preserve original source and destination IP addresses.
- Prevent overlapping network conflicts.
- Provide a scalable enterprise routing design.

---

# Network Addressing

## AWS

| Network | CIDR |
|----------|------|
| VPC | 10.0.0.0/16 |
| Public Subnet | 10.0.1.0/24 |
| WireGuard Tunnel | 172.168.100.1/24 |

---

## Azure

| Network | CIDR |
|----------|------|
| VNet | 10.1.0.0/16 |
| Public Subnet | 10.1.1.0/24 |
| WireGuard Tunnel | 172.168.100.2/24 |

---

# Routing Topology

```
                 AWS VPC
              10.0.0.0/16
                    │
                    │
          WireGuard Gateway
          172.168.100.1
                    │
══════════════════════════════════════
      Encrypted WireGuard Tunnel
══════════════════════════════════════
                    │
          172.168.100.2
          WireGuard Gateway
                    │
                    │
              Azure VNet
              10.1.0.0/16
```

---

# AWS Routing

The AWS gateway routes traffic destined for Azure through the WireGuard interface.

## Local Routes

| Destination | Purpose |
|-------------|---------|
| 10.0.0.0/16 | Local AWS VPC |

## Remote Routes

| Destination | Interface |
|-------------|-----------|
| 10.1.0.0/16 | wg0 |

Traffic destined for Azure is encrypted before leaving the AWS gateway.

---

# Azure Routing

The Azure gateway routes traffic destined for AWS through the WireGuard interface.

## Local Routes

| Destination | Purpose |
|-------------|---------|
| 10.1.0.0/16 | Local Azure VNet |

## Remote Routes

| Destination | Interface |
|-------------|-----------|
| 10.0.0.0/16 | wg0 |

Traffic destined for AWS is encrypted before leaving the Azure gateway.

---

# WireGuard Routing

WireGuard automatically installs routes based on the `AllowedIPs` configuration.

## AWS

```conf
AllowedIPs = 10.1.0.0/16,172.168.100.2/32
```

This creates routes for:

- Azure Virtual Network
- Azure WireGuard tunnel interface

---

## Azure

```conf
AllowedIPs = 10.0.0.0/16,172.168.100.1/32
```

This creates routes for:

- AWS Virtual Private Cloud
- AWS WireGuard tunnel interface

---

# IP Forwarding

Both VPN gateways function as routers.

IPv4 forwarding is enabled on each Linux gateway.

```conf
net.ipv4.ip_forward=1
```

This allows packets arriving on one interface to be forwarded to another interface.

Without IP forwarding, the VPN tunnel would establish successfully, but traffic would not traverse between the AWS and Azure private networks.

---

# Routing Process

## AWS to Azure

1. AWS resource sends traffic to an Azure private IP.
2. Linux routing table forwards traffic to `wg0`.
3. WireGuard encrypts the packet.
4. Packet traverses the Internet.
5. Azure WireGuard gateway decrypts the packet.
6. Azure forwards the packet to the destination VM.

---

## Azure to AWS

1. Azure resource sends traffic to an AWS private IP.
2. Linux routing table forwards traffic to `wg0`.
3. WireGuard encrypts the packet.
4. Packet traverses the Internet.
5. AWS WireGuard gateway decrypts the packet.
6. AWS forwards the packet to the destination EC2 instance.

---

# Routing Without NAT

This implementation intentionally uses routed networking instead of Network Address Translation (NAT).

Reasons include:

- Non-overlapping private networks
- End-to-end visibility of source IP addresses
- Simpler troubleshooting
- Improved logging and auditing
- Enterprise networking best practices

AWS Network

```
10.0.0.0/16
```

Azure Network

```
10.1.0.0/16
```

Since these networks do not overlap, address translation is unnecessary.

---

# Route Verification

Routes can be viewed with:

```bash
ip route
```

WireGuard interface:

```bash
ip addr show wg0
```

Tunnel status:

```bash
sudo wg
```

---

# Connectivity Verification

Successful routing was verified through the following tests.

## Tunnel Connectivity

AWS

```bash
ping 172.168.100.2
```

Azure

```bash
ping 172.168.100.1
```

---

## Private Network Connectivity

AWS

```bash
ping 10.1.1.4
```

Azure

```bash
ping 10.0.1.40
```

All tests completed successfully.

---

# Verification Screenshots

```
images/testing/verification/
```

Included screenshots demonstrate:

- WireGuard handshake
- Tunnel connectivity
- AWS to Azure communication
- Azure to AWS communication
- Private network routing

---

# Design Benefits

The routing design provides:

- Secure encrypted communication
- Cloud-to-cloud connectivity
- End-to-end private networking
- No Network Address Translation
- Simplified troubleshooting
- Enterprise scalability
- Low-latency communication

---

# Skills Demonstrated

- Linux Routing
- IP Forwarding
- WireGuard VPN
- AWS Networking
- Azure Networking
- Hybrid Cloud Architecture
- TCP/IP
- Route Tables
- Network Verification
- Infrastructure Documentation

---

# Outcome

A routed site-to-site VPN was successfully implemented between AWS and Azure.

Traffic destined for remote private networks is automatically forwarded through the WireGuard tunnel, allowing resources in both cloud environments to communicate securely without exposing internal traffic to the public Internet or requiring Network Address Translation.