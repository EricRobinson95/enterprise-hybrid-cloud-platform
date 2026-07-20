# 10 - Routing

# Overview

The Enterprise Hybrid Cloud Platform uses static IPv4 routing to provide secure communication between Amazon Web Services (AWS), Microsoft Azure, and the on-premises security lab.

All inter-site traffic traverses encrypted WireGuard VPN tunnels using a **hub-and-spoke topology**, with AWS acting as the central routing hub.

---

# Routing Objectives

- Provide secure communication between all connected environments
- Centralize routing through AWS
- Maintain isolated network segments
- Prevent overlapping address spaces
- Support future enterprise services and application deployment
- Simulate a real-world hybrid cloud network

---

# Network Addressing

| Environment | Network |
|------------|---------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# Routing Architecture

```
                    AWS Hub
                 10.0.0.0/16
              WireGuard Gateway
               172.16.100.1
                  /       \
                 /         \
                /           \
               ▼             ▼

Azure                    On-Premises
10.1.0.0/16             10.2.0.0/16
172.16.100.2            172.16.100.3
```

AWS functions as the central routing point for all VPN-connected environments.

---

# Route Advertisement

Each WireGuard gateway advertises its local network.

| Gateway | Advertised Networks |
|----------|---------------------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |

---

# AWS Routing

AWS serves as the hub for the hybrid cloud environment.

## Responsibilities

- Route traffic between VPN-connected environments
- Forward packets through WireGuard
- Maintain VPN connectivity
- Provide access to production services

### Linux Routes

```
10.1.0.0/16 dev wg0

10.2.0.0/16 dev wg0

172.16.100.0/24 dev wg0

default via 10.0.1.1 dev ens5
```

---

# Azure Routing

Azure routes remote traffic through the WireGuard gateway.

### Linux Routes

```
10.0.0.0/16 dev wg0

172.16.100.0/24 dev wg0

default via Azure Virtual Network Gateway
```

Azure advertises:

```
10.1.0.0/16
```

---

# On-Premises Routing

The Ubuntu WireGuard gateway routes traffic between the on-premises security lab and AWS.

### Linux Routes

```
10.0.0.0/16 dev wg0

172.16.100.0/24 dev wg0

default via Local Gateway
```

The gateway advertises:

```
10.2.0.0/16
```

---

# IP Forwarding

WireGuard gateways perform Layer 3 forwarding between local interfaces and the VPN tunnel.

Linux IP forwarding is enabled on all VPN gateways.

Verification

```bash
sysctl net.ipv4.ip_forward
```

Expected output

```
net.ipv4.ip_forward = 1
```

---

# Route Verification

Linux routing tables are verified using:

```bash
ip route
```

Example

```
10.1.0.0/16 dev wg0

10.2.0.0/16 dev wg0

172.16.100.0/24 dev wg0
```

---

# Route Lookup

Individual route selection can be verified using:

```bash
ip route get <destination-ip>
```

Example

```bash
ip route get 10.1.1.4
```

---

# Path Verification

Routing paths are validated using:

```bash
traceroute <destination>
```

Example

```bash
traceroute 10.1.1.4
```

This confirms the path selected by the Linux routing table.

---

# Packet Verification

Traffic is verified using:

```bash
sudo tcpdump
```

Examples

Capture ICMP

```bash
sudo tcpdump -ni wg0 icmp
```

Capture WireGuard

```bash
sudo tcpdump -ni any udp port 51820
```

Capture all traffic

```bash
sudo tcpdump -ni any
```

---

# Current Routing Status

## Operational

✓ AWS ↔ Azure

✓ AWS ↔ On-Premises

✓ Static routing

✓ Linux routing

✓ WireGuard routing

✓ VPN tunnel routing

---

# Current Limitation

The current implementation uses a **hub-and-spoke** routing model.

AWS acts as the central routing hub.

Communication between:

- AWS ↔ Azure
- AWS ↔ On-Premises

is fully operational.

Direct Azure ↔ On-Premises transit routing through the AWS hub has not been implemented and remains a future enhancement.

This limitation does not affect the planned architecture because:

- Azure hosts enterprise identity and developer services.
- AWS hosts the production application.
- The on-premises environment performs security testing against AWS-hosted resources.

---

# Routing Design Decisions

The project intentionally uses static routing because it:

- Simplifies configuration
- Reduces operational complexity
- Demonstrates enterprise network fundamentals
- Is appropriate for a small hybrid cloud deployment

Dynamic routing protocols such as BGP are planned as a future enhancement.

---

# Future Enhancements

- BGP Dynamic Routing
- Route Redundancy
- High Availability VPN Gateways
- ECMP
- Automated Route Deployment
- Infrastructure as Code
- VPN Monitoring
- Route Health Monitoring

---

# Summary

The Enterprise Hybrid Cloud Platform uses static routing and a WireGuard hub-and-spoke VPN architecture to securely connect AWS, Azure, and the on-premises security lab.

AWS serves as the central routing hub, enabling secure communication between production infrastructure, corporate services, and the security testing environment while providing a scalable foundation for future enterprise expansion.