# Subnet Design Lab: Fulfillment Center Network Segmentation

## Purpose

This lab exercise demonstrates manual subnetting by dividing a large IP address block into functional segments for a generic fulfillment center environment. Subnetting is a core networking skill for IT support engineers — it determines how devices are grouped, how traffic is isolated, and how address space is conserved.

This is a learning-lab exercise created for Amazon IT Support Engineer I (Ops Tech Solutions) preparation. It does not represent a real production network design.

---

## Scenario

A fulfillment center has been assigned the private IP block **10.0.0.0/16**, providing 65,534 usable host addresses. The facility needs to segment this space into dedicated subnets for different operational zones, each with appropriate capacity and isolation.

### Requirements

| Zone | Purpose | Estimated Devices | Isolation Reason |
|------|---------|-------------------|------------------|
| Operations Workstations | Pack stations, problem-solve desks, manager laptops | ~200 | General employee traffic separated from automation |
| Handheld Scanners | Pick, stow, and count scanners on Wi-Fi | ~500 | High-density wireless traffic isolated from wired infrastructure |
| Label Printers | Thermal printers at pack and ship stations | ~100 | Print traffic kept separate to prevent broadcast interference |
| Robotics and Automation | Conveyor controllers, robotic drive units, PLCs | ~250 | Safety-critical systems fully isolated from user traffic |
| Management and IT | IT admin workstations, monitoring servers, network management | ~30 | Restricted access for infrastructure administration |
| Guest and Visitor Wi-Fi | Contractor and visitor internet access | ~50 | No access to internal resources; internet-only |

---

## Subnet Design

### Approach: Fixed-Length Subnetting

For this exercise, I am using /24 subnets (256 addresses, 254 usable hosts each) for simplicity and consistency. In a real environment, Variable Length Subnet Masking (VLSM) would optimize address usage, but fixed-length subnetting builds the foundational understanding first.

### Address Allocation Table

| Subnet Name | Network Address | CIDR | Subnet Mask | Usable Range | Broadcast | Usable Hosts | Assigned Zone |
|---|---|---|---|---|---|---|---|
| Subnet A | 10.0.1.0 | /24 | 255.255.255.0 | 10.0.1.1 – 10.0.1.254 | 10.0.1.255 | 254 | Operations Workstations |
| Subnet B | 10.0.2.0 | /24 | 255.255.255.0 | 10.0.2.1 – 10.0.2.254 | 10.0.2.255 | 254 | Handheld Scanners |
| Subnet C | 10.0.3.0 | /24 | 255.255.255.0 | 10.0.3.1 – 10.0.3.254 | 10.0.3.255 | 254 | Label Printers |
| Subnet D | 10.0.4.0 | /24 | 255.255.255.0 | 10.0.4.1 – 10.0.4.254 | 10.0.4.255 | 254 | Robotics and Automation |
| Subnet E | 10.0.5.0 | /24 | 255.255.255.0 | 10.0.5.1 – 10.0.5.254 | 10.0.5.255 | 254 | Management and IT |
| Subnet F | 10.0.6.0 | /24 | 255.255.255.0 | 10.0.6.1 – 10.0.6.254 | 10.0.6.255 | 254 | Guest and Visitor Wi-Fi |

### Default Gateway Convention

Each subnet uses the .1 address as the default gateway (the router interface for that segment):
- 10.0.1.1 — Gateway for Operations Workstations
- 10.0.2.1 — Gateway for Handheld Scanners
- 10.0.3.1 — Gateway for Label Printers
- 10.0.4.1 — Gateway for Robotics and Automation
- 10.0.5.1 — Gateway for Management and IT
- 10.0.6.1 — Gateway for Guest Wi-Fi

---

## VLSM Optimization (Advanced Exercise)

In practice, not every zone needs 254 addresses. VLSM allows each subnet to be sized to its actual requirement, conserving address space.

| Zone | Devices Needed | Minimum Subnet Size | CIDR | Subnet Mask | Usable Hosts |
|------|---------------|---------------------|------|-------------|--------------|
| Handheld Scanners | ~500 | /23 (512 addresses) | /23 | 255.255.254.0 | 510 |
| Robotics and Automation | ~250 | /24 (256 addresses) | /24 | 255.255.255.0 | 254 |
| Operations Workstations | ~200 | /24 (256 addresses) | /24 | 255.255.255.0 | 254 |
| Label Printers | ~100 | /25 (128 addresses) | /25 | 255.255.255.128 | 126 |
| Guest Wi-Fi | ~50 | /26 (64 addresses) | /26 | 255.255.255.192 | 62 |
| Management and IT | ~30 | /27 (32 addresses) | /27 | 255.255.255.224 | 30 |

### VLSM Allocation (starting from 10.0.0.0/16)

Allocate largest subnets first to avoid fragmentation:

| Zone | Network Address | CIDR | Usable Range | Usable Hosts |
|------|----------------|------|--------------|--------------|
| Handheld Scanners | 10.0.0.0 | /23 | 10.0.0.1 – 10.0.1.254 | 510 |
| Robotics and Automation | 10.0.2.0 | /24 | 10.0.2.1 – 10.0.2.254 | 254 |
| Operations Workstations | 10.0.3.0 | /24 | 10.0.3.1 – 10.0.3.254 | 254 |
| Label Printers | 10.0.4.0 | /25 | 10.0.4.1 – 10.0.4.126 | 126 |
| Guest Wi-Fi | 10.0.4.128 | /26 | 10.0.4.129 – 10.0.4.190 | 62 |
| Management and IT | 10.0.4.192 | /27 | 10.0.4.193 – 10.0.4.222 | 30 |

**Total addresses used:** 1,236 out of 65,534 available — significant conservation compared to six /24 blocks (1,524 allocated).

---

## Subnetting Math Reference

### How to Calculate Usable Hosts

Formula: **2^(host bits) - 2 = usable hosts**

The -2 accounts for the network address (all host bits = 0) and broadcast address (all host bits = 1).

| CIDR | Host Bits | Total Addresses | Usable Hosts |
|------|-----------|-----------------|--------------|
| /24 | 8 | 256 | 254 |
| /25 | 7 | 128 | 126 |
| /26 | 6 | 64 | 62 |
| /27 | 5 | 32 | 30 |
| /28 | 4 | 16 | 14 |
| /23 | 9 | 512 | 510 |
| /22 | 10 | 1,024 | 1,022 |

### How to Identify Network and Broadcast Addresses

For a /26 subnet (block size = 64):
- Subnets start at multiples of 64: .0, .64, .128, .192
- Network address: first address in the block (e.g., 10.0.4.128)
- Broadcast address: last address in the block (e.g., 10.0.4.191)
- Usable range: everything between (10.0.4.129 – 10.0.4.190)

---

## Design Decisions and Rationale

### Why Isolate Robotics?
Robotic drive units and conveyor controllers generate high-frequency, time-sensitive traffic. Placing them on a shared subnet with user workstations risks broadcast storms affecting safety-critical automation. Isolation ensures predictable performance.

### Why Separate Scanners from Workstations?
Handheld scanners are wireless, high-density, and mobile. Their traffic patterns (short bursts, frequent roaming between access points) differ significantly from wired workstations. Separate subnets allow different QoS policies and DHCP lease configurations.

### Why Restrict Guest Wi-Fi?
Guest and visitor traffic must never reach internal resources. A dedicated subnet with no routes to internal subnets (only a default route to the internet) enforces this boundary at the network layer.

### Why a Small Management Subnet?
IT administration traffic (SSH to switches, SNMP monitoring, firmware updates) should be restricted to authorized personnel. A small /27 subnet limits the attack surface and makes access control lists simpler to manage.

---

## Troubleshooting Connections

| Subnetting Concept | Related Troubleshooting Scenario |
|---|---|
| Device gets 169.254.x.x (APIPA) | DHCP server unreachable or scope exhausted for this subnet |
| Device cannot reach another subnet | Default gateway misconfigured or missing route between subnets |
| Broadcast storm affecting performance | Subnet too large; too many devices in one broadcast domain |
| New device cannot get IP address | Subnet's DHCP scope has no remaining leases (address exhaustion) |
| Scanner works in one zone but not another | Device roamed to a different subnet and did not get a new DHCP lease |

---

## What I Learned

- Subnetting is not just math — it is a design decision that affects security, performance, and troubleshooting efficiency
- Fixed-length subnetting (/24 everywhere) is simple but wasteful; VLSM requires more planning but conserves address space significantly
- The largest subnets should be allocated first in VLSM to avoid address fragmentation
- Every subnet design decision has an operational reason (isolation, capacity, security, performance)
- Understanding subnet boundaries is essential for diagnosing "device can reach local but not remote" issues

---

## Resume / Interview Connection

- **IP addressing and subnetting:** Demonstrates manual calculation of network addresses, broadcast addresses, usable ranges, and CIDR notation without relying on online calculators
- **Network design thinking:** Shows ability to translate operational requirements (device counts, isolation needs, security boundaries) into a structured subnet plan
- **OTS relevance:** Directly maps to maintaining uptime and availability — understanding subnet design helps diagnose address exhaustion, VLAN mismatches, and routing failures in high-density FC environments
