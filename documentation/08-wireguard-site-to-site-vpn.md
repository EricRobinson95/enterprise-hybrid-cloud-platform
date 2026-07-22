# WireGuard Site-to-Site VPN

## Overview

This document describes the implementation of the WireGuard site-to-site VPN used within the Enterprise Hybrid Cloud Platform.

The VPN securely connects Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware environment using a hub-and-spoke topology.

AWS functions as the central VPN hub while Azure and the on-premises environment operate as spoke sites.

The VPN provides encrypted communication between all three private networks while allowing each environment to maintain its own routing domain.

---

# Objectives

The WireGuard deployment provides

- Secure hybrid cloud connectivity
- Encrypted site-to-site communication
- Centralized routing
- Secure remote administration
- Cross-cloud communication
- On-premises integration
- Enterprise scalability

---

# VPN Topology

```
                    Internet
                        │
              Encrypted WireGuard VPN
                        │
               AWS WireGuard Hub
                172.16.100.1
              AWS VPC 10.0.0.0/16
                 /             \
                /               \
               /                 \
      Azure Spoke           On-Premises Spoke
      172.16.100.2          172.16.100.3
    Azure 10.1.0.0/16      VMware 10.2.0.0/16
```

---

# VPN Networks

| Environment | Network |
|------------|------------|
| AWS | 10.0.0.0/16 |
| Azure | 10.1.0.0/16 |
| On-Premises | 10.2.0.0/16 |
| WireGuard Tunnel | 172.16.100.0/24 |

---

# WireGuard Tunnel Addresses

| Gateway | Tunnel Address |
|----------|----------------|
| AWS Hub | 172.16.100.1 |
| Azure Gateway | 172.16.100.2 |
| On-Premises Gateway | 172.16.100.3 |

---

# WireGuard Configuration

## AWS Hub

```ini
[Interface]
Address = 172.16.100.1
PrivateKey = <AWS_PRIVATE_KEY>
ListenPort = 60031

# Azure
[Peer]
PublicKey = <AZURE_PUBLIC_KEY>
Endpoint = <AZURE_PUBLIC_IP>:60031

AllowedIPs = 10.1.0.0/16,172.16.100.2/32

PersistentKeepalive = 25

# On-Premises
[Peer]
PublicKey = <ONPREM_PUBLIC_KEY>
Endpoint = <ONPREM_PUBLIC_IP>:60032

AllowedIPs = 10.2.0.0/16,172.16.100.3/32

PersistentKeepalive = 25
```

---

## Azure Spoke

```ini
[Interface]
Address = 172.16.100.2
PrivateKey = <AZURE_PRIVATE_KEY>
ListenPort = 60031

[Peer]
PublicKey = <AWS_PUBLIC_KEY>
Endpoint = <AWS_PUBLIC_IP>:60031

AllowedIPs = 10.0.0.0/16,10.2.0.0/16,172.16.100.1/32,172.16.100.3/32

PersistentKeepalive = 25
```

---

## On-Premises Spoke

```ini
[Interface]
Address = 172.16.100.3
PrivateKey = <ONPREM_PRIVATE_KEY>
ListenPort = 60032

[Peer]
PublicKey = <AWS_PUBLIC_KEY>
Endpoint = <AWS_PUBLIC_IP>:60031

AllowedIPs = 10.0.0.0/16,10.1.0.0/16,172.16.100.1/32,172.16.100.2/32

PersistentKeepalive = 25
```

---

# Routing

## AWS Hub

Routes

```
10.1.0.0/16 → wg0

10.2.0.0/16 → wg0
```

---

## Azure

Routes

```
10.0.0.0/16 → wg0

10.2.0.0/16 → wg0
```

---

## On-Premises

Routes

```
10.0.0.0/16 → wg0

10.1.0.0/16 → wg0
```

---

# Linux Configuration

## Enable IP Forwarding

```
net.ipv4.ip_forward=1
```

---

## Verify

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Expected Output

```
1
```

---

# iptables

Forwarding

```bash
iptables -A FORWARD -i wg0 -j ACCEPT

iptables -A FORWARD -o wg0 -j ACCEPT
```

---

NAT

```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

---

# WireGuard Service

Start

```bash
sudo systemctl start wg-quick@wg0
```

Enable

```bash
sudo systemctl enable wg-quick@wg0
```

Restart

```bash
sudo systemctl restart wg-quick@wg0
```

Status

```bash
sudo systemctl status wg-quick@wg0
```

---

# Verification Commands

Check peers

```bash
wg
```

Routing

```bash
ip route
```

Interface

```bash
ip addr
```

Forwarding

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Firewall

```bash
iptables -L

iptables -t nat -L
```

Packet Capture

```bash
tcpdump -ni wg0
```

---

# Connectivity Validation

The following connectivity has been verified.

✓ AWS ↔ Azure

✓ AWS ↔ On-Premises

✓ Azure ↔ On-Premises

✓ On-Premises ↔ Azure

✓ WireGuard handshakes

✓ Static routing

✓ Encrypted traffic

✓ Packet forwarding

---

# Troubleshooting

## Problem

WireGuard tunnels successfully established handshakes, but Azure and the On-Premises environment could not communicate through the AWS hub.

---

## Investigation

The following components were verified during troubleshooting.

- WireGuard peers
- Public keys
- Private keys
- Tunnel endpoints
- Security Groups
- Azure Network Security Groups
- Linux routing
- Static routes
- IP forwarding
- iptables forwarding
- NAT
- Source/Destination Check
- Packet forwarding
- tcpdump captures

---

## Root Cause

The Azure and On-Premises WireGuard peers did not initially include the opposite spoke's tunnel IP address in their `AllowedIPs` configuration.

Although the VPN handshakes were successful, WireGuard silently rejected forwarded packets because the tunnel source addresses were not permitted.

---

## Resolution

Updated Azure

```
AllowedIPs

10.0.0.0/16
10.2.0.0/16
172.16.100.1/32
172.16.100.3/32
```

Updated On-Premises

```
AllowedIPs

10.0.0.0/16
10.1.0.0/16
172.16.100.1/32
172.16.100.2/32
```

After updating the configuration and restarting WireGuard, full end-to-end connectivity between Azure and the on-premises environment was restored.

---

# Lessons Learned

During implementation the following concepts were reinforced.

- WireGuard handshakes do not guarantee successful routing.
- `AllowedIPs` functions as both a routing table and an access control list.
- Linux IP forwarding must be enabled on VPN routers.
- AWS EC2 Source/Destination Check must be disabled when acting as a router.
- Packet captures with `tcpdump` are invaluable for diagnosing VPN routing issues.
- A structured troubleshooting approach is more effective than changing multiple settings at once.

---

# Current Status

| Component | Status |
|-----------|--------|
| AWS WireGuard Hub | Complete |
| Azure WireGuard Spoke | Complete |
| On-Premises WireGuard Spoke | Complete |
| Site-to-Site VPN | Complete |
| Hub-and-Spoke Routing | Complete |
| Linux Routing | Complete |
| End-to-End Connectivity | Complete |
| VPN Validation | Complete |

---

# Future Improvements

Future enhancements include

- High Availability VPN gateways
- Dynamic routing with BGP
- Dual AWS VPN hubs
- Azure VPN Gateway comparison
- Infrastructure automation with Terraform
- Automated deployment with Ansible
- VPN monitoring using Prometheus and Grafana