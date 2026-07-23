# 14 -  Troubleshooting

# Overview

Throughout the implementation of the Enterprise Hybrid Cloud Platform, several technical challenges were encountered while integrating Amazon Web Services (AWS), Microsoft Azure, and an on-premises VMware environment.

Each issue required investigation, troubleshooting, and validation before the project could progress. This document summarizes the most significant problems encountered, their root causes, and the solutions implemented.

---

# Troubleshooting Methodology

Every issue was approached using the same troubleshooting process.

1. Identify the problem.
2. Verify the current configuration.
3. Isolate the affected component.
4. Review logs and network configuration.
5. Test individual components.
6. Implement a solution.
7. Validate the fix.
8. Document the outcome.

This systematic approach ensured that changes were verified before moving to the next implementation phase.

---

# Issue 1 – WireGuard Tunnel Established but No Connectivity

## Symptoms

- WireGuard peers successfully established handshakes.
- `wg` displayed active peers.
- End-to-end communication failed.
- Ping requests timed out.

---

## Root Cause

Although the VPN tunnels were operational, the WireGuard `AllowedIPs` configuration on the spoke gateways did not include all remote tunnel networks.

Traffic was successfully encrypted but was not being routed correctly.

---

## Resolution

Updated the WireGuard configuration on each gateway.

Validated:

- AllowedIPs
- Peer configuration
- Tunnel endpoints

Restarted the WireGuard service.

---

## Verification

```bash
wg

ping

tcpdump
```

Result

✅ End-to-end communication restored.

---

# Issue 2 – Linux Packet Forwarding Disabled

## Symptoms

- VPN tunnel established.
- Local communication successful.
- Remote networks unreachable.

---

## Root Cause

Linux IP forwarding was disabled.

The Ubuntu gateways were functioning as VPN endpoints but were not forwarding packets between interfaces.

---

## Resolution

Enabled IPv4 forwarding.

```bash
sysctl -w net.ipv4.ip_forward=1
```

Made the configuration persistent.

---

## Verification

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Result

✅ Packet forwarding enabled.

---

# Issue 3 – Static Routing Problems

## Symptoms

- Hosts could reach the VPN gateway.
- Remote private networks remained unreachable.

---

## Root Cause

Static routes were missing or pointed to incorrect gateways.

---

## Resolution

Reviewed routing tables on:

- AWS
- Azure
- Ubuntu Gateways
- Windows Workstation

Added the required static routes.

---

## Verification

```bash
ip route
```

```powershell
route print
```

Result

✅ Cross-site routing operational.

---

# Issue 4 – AWS Source/Destination Check

## Symptoms

Traffic entered the AWS gateway but was never forwarded.

---

## Root Cause

AWS EC2 instances perform source and destination checking by default.

This prevents an instance from acting as a router.

---

## Resolution

Disabled Source/Destination Check on the AWS WireGuard gateway.

---

## Verification

AWS Console

EC2 Instance

Networking

Source/Destination Check

Result

✅ AWS gateway successfully forwarded traffic.

---

# Issue 5 – Azure User Defined Routes

## Symptoms

Azure virtual machines could not reach remote networks.

---

## Root Cause

Azure traffic was using the default system routes instead of the Ubuntu VPN gateway.

---

## Resolution

Configured:

- User Defined Route Table
- Next Hop
- Route Association

---

## Verification

Azure Route Tables

Successful ping tests

Result

✅ Azure traffic routed through WireGuard gateway.

---

# Issue 6 – Windows Persistent Routes

## Symptoms

Windows workstation could reach Azure resources but not every remote subnet after reboot.

---

## Root Cause

Routes added using the Windows `route add` command were not persistent.

---

## Resolution

Created persistent static routes.

```powershell
route -p add
```

---

## Verification

```powershell
route print
```

Result

✅ Routes persisted after restart.

---

# Issue 7 – Active Directory Domain Join

## Symptoms

Windows 11 could not initially join the Active Directory domain.

---

## Root Cause

DNS was not pointing to the Azure domain controller.

Active Directory relies entirely on DNS.

---

## Resolution

Configured the workstation to use the Azure DNS server.

Validated DNS resolution before joining the domain.

---

## Verification

```powershell
nslookup

ping

systeminfo
```

Result

✅ Successful domain join.

---

# Issue 8 – Cross-Site Authentication

## Symptoms

Users could not authenticate from the on-premises workstation.

---

## Root Cause

Authentication traffic could not reach the Azure domain controller because VPN routing was incomplete.

---

## Resolution

Verified:

- VPN tunnels
- Static routes
- DNS
- Firewall rules

---

## Verification

```powershell
whoami
```

```powershell
hostname
```

Domain login completed successfully.

Result

✅ Hybrid authentication operational.

---

# Issue 9 – Remote Desktop Access

## Symptoms

Administrative users could successfully connect through Remote Desktop while newly created enterprise accounts could not.

---

## Root Cause

User permissions and group membership required verification after account creation.

---

## Resolution

Validated:

- Active Directory user properties
- Security group membership
- Remote Desktop permissions
- User logon configuration

Retested authentication.

---

## Verification

Remote Desktop login

```powershell
whoami
```

Result

✅ Enterprise user successfully authenticated.

---

# Issue 10 – SSH Administration

## Symptoms

SSH connections occasionally failed during initial deployment.

---

## Root Cause

Configuration issues included:

- SSH service
- Firewall rules
- Incorrect IP addresses
- Incorrect private key usage

---

## Resolution

Verified:

```bash
systemctl status ssh

ip addr

ufw status
```

Retested connectivity.

Result

✅ SSH administration restored.

---

# Issue 11 – Documentation Accuracy

## Symptoms

As the infrastructure evolved, diagrams, screenshots, and documentation became outdated.

---

## Resolution

Performed a complete documentation review.

Updated:

- Architecture diagrams
- IP addressing
- Active Directory
- Network architecture
- Validation screenshots
- Project roadmap
- README
- Supporting documentation

---

# Validation Tools

The following tools were used throughout troubleshooting.

Linux

```bash
wg

ip addr

ip route

ping

tcpdump

systemctl

journalctl
```

Windows

```powershell
whoami

hostname

systeminfo

route print

nslookup

gpresult
```

Cloud Platforms

- AWS Console
- Azure Portal
- VMware Workstation
- Windows Admin Tools

---

# Lessons Learned

The project reinforced several important enterprise infrastructure concepts.

- VPN connectivity does not guarantee routing.
- Active Directory depends on reliable DNS.
- Linux routers require IP forwarding.
- Static routing must be verified on every network.
- AWS routing requires Source/Destination Check to be disabled for router instances.
- Azure networking depends on correctly configured User Defined Routes.
- Network validation should occur after every infrastructure change.
- Comprehensive documentation greatly simplifies troubleshooting and future maintenance.

---

# Summary

Building the Enterprise Hybrid Cloud Platform required troubleshooting across cloud networking, VPN technologies, Linux administration, Windows Server administration, Active Directory, DNS, routing, and hybrid authentication.

Each issue was systematically identified, isolated, resolved, validated, and documented. The troubleshooting experience gained throughout the project closely reflects the day-to-day responsibilities of an Infrastructure Engineer, Systems Engineer, or Cloud Engineer responsible for deploying and maintaining enterprise hybrid cloud environments.