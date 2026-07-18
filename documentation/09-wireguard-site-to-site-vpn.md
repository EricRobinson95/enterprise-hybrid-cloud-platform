# 09 - WireGuard Site-to-Site VPN

# Overview

This section documents the deployment and configuration of a secure site-to-site Virtual Private Network (VPN) between Amazon Web Services (AWS) and Microsoft Azure using WireGuard.

The VPN creates an encrypted tunnel between the AWS Virtual Private Cloud (VPC) and the Azure Virtual Network (VNet), allowing private communication between cloud resources without exposing internal traffic to the public Internet.

---

# Objectives

- Install WireGuard on both Linux virtual machines.
- Generate secure public/private key pairs.
- Configure encrypted peer-to-peer communication.
- Route private network traffic through the VPN.
- Verify encrypted connectivity between AWS and Azure.
- Document the complete deployment process.

---

# Environment

## AWS

| Resource | Value |
|----------|-------|
| Cloud Provider | Amazon Web Services |
| Operating System | Ubuntu Server 24.04 LTS |
| Private Network | 10.0.0.0/16 |
| WireGuard Address | 172.168.100.1/24 |
| Listen Port | 51820 |

---

## Azure

| Resource | Value |
|----------|-------|
| Cloud Provider | Microsoft Azure |
| Operating System | Ubuntu Server 24.04 LTS |
| Private Network | 10.1.0.0/16 |
| WireGuard Address | 172.168.100.2/24 |
| Listen Port | 51820 |

---

# VPN Tunnel

Tunnel Network

```
172.168.100.0/24
```

AWS Endpoint

```
<AWS Elastic IP>:51820
```

Azure Endpoint

```
<Azure Static Public IP>:51820
```

---

# Software Installation

WireGuard was installed on both Ubuntu virtual machines.

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install wireguard -y
```

---

# Key Generation

Each VPN gateway generated a unique WireGuard key pair.

```bash
umask 077
wg genkey | tee privatekey | wg pubkey > publickey
```

This generated:

- Private Key
- Public Key

Each server exchanged **only the public key**.

Private keys remained securely stored on their respective virtual machines.

---

# WireGuard Configuration

The configuration file is stored at:

```text
/etc/wireguard/wg0.conf
```

---

## AWS Configuration

```conf
[Interface]
Address = 172.168.100.1/24
PrivateKey = <AWS Private Key>
ListenPort = 51820

[Peer]
PublicKey = <Azure Public Key>
Endpoint = <Azure Public IP>:51820
AllowedIPs = 10.1.0.0/16,172.168.100.2/32
PersistentKeepalive = 25
```

---

## Azure Configuration

```conf
[Interface]
Address = 172.168.100.2/24
PrivateKey = <Azure Private Key>
ListenPort = 51820

[Peer]
PublicKey = <AWS Public Key>
Endpoint = <AWS Elastic IP>:51820
AllowedIPs = 10.0.0.0/16,172.168.100.1/32
PersistentKeepalive = 25
```

---

# Enable IP Forwarding

IP forwarding was enabled on both Linux gateways.

`/etc/sysctl.conf`

```conf
net.ipv4.ip_forward=1
net.ipv6.conf.all.forwarding=1
```

Apply the configuration:

```bash
sudo sysctl -p
```

---

# Starting the VPN

Bring up the WireGuard interface.

```bash
sudo wg-quick up wg0
```

Verify the interface.

```bash
ip addr
```

---

# Verify Tunnel Status

Display tunnel information.

```bash
sudo wg
```

Expected output includes:

- Interface status
- Peer information
- Latest handshake
- Data transferred
- Persistent keepalive

---

# Connectivity Testing

## Verify WireGuard Tunnel

AWS

```bash
ping 172.168.100.2
```

Azure

```bash
ping 172.168.100.1
```

---

## Verify AWS → Azure Private Network

```bash
ping 10.1.1.4
```

---

## Verify Azure → AWS Private Network

```bash
ping 10.0.1.40
```

---

# Verification Results

The following tests completed successfully.

- WireGuard interface active on AWS
- WireGuard interface active on Azure
- Successful peer handshake
- Tunnel established
- Encrypted packet transfer
- AWS reached Azure WireGuard interface
- Azure reached AWS WireGuard interface
- AWS reached Azure private subnet
- Azure reached AWS private subnet

---

# Verification Screenshots

```
images/testing/verification/
│
├── aws-wireguard-status.png
├── azure-wireguard-status.png
├── aws-to-azure-wireguard-ping.png
├── azure-to-aws-wireguard-ping.png
├── aws-to-azure-private-network.png
└── azure-to-aws-private-network.png
```

---

# Security Considerations

- SSH access restricted using cloud firewall rules when possible.
- WireGuard uses modern public-key cryptography.
- Private keys are never committed to GitHub.
- Configuration files stored in the repository use placeholder values.
- All communication between cloud environments is encrypted.

---

# Files

```
configs/
├── linux/
│   └── sysctl.conf
│
└── wireguard/
    ├── aws.conf
    └── azure.conf
```

---

# Skills Demonstrated

- Hybrid Cloud Networking
- AWS Networking
- Azure Networking
- WireGuard VPN
- Linux Administration
- SSH
- Network Routing
- Public Key Cryptography
- IP Addressing
- Infrastructure Documentation
- Network Verification

---

# Outcome

A fully functional WireGuard site-to-site VPN was successfully deployed between AWS and Azure.

The VPN provides secure encrypted communication between both cloud providers, enabling private resources within each network to communicate across the Internet without exposing internal traffic. Successful tunnel establishment, peer handshakes, and bidirectional connectivity tests verified the implementation.