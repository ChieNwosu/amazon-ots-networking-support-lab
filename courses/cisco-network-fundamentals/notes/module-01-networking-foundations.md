# Module 01: Networking Foundations

## Purpose

This module establishes the foundational concepts that underpin all network communication. Understanding how devices connect, how data traverses a network, and how the OSI and TCP/IP models organize network functions is essential before any meaningful troubleshooting can occur. For an OTS engineer, this is the knowledge base that makes structured diagnostics possible.

---

## Key Concepts

### The OSI Model (7 Layers)

The Open Systems Interconnection model provides a framework for understanding how network communication occurs at each layer:

| Layer | Name | Function | OTS Relevance |
|-------|------|----------|---------------|
| 7 | Application | User-facing protocols (HTTP, DNS, DHCP) | Where users experience failures |
| 6 | Presentation | Data formatting, encryption | SSL/TLS issues, encoding problems |
| 5 | Session | Session management, connections | Application timeout issues |
| 4 | Transport | Reliable delivery (TCP/UDP), port numbers | Port-based troubleshooting, firewall rules |
| 3 | Network | IP addressing, routing | Ping, traceroute, routing issues |
| 2 | Data Link | MAC addressing, switching, VLANs | Switch port issues, VLAN assignment |
| 1 | Physical | Cables, signals, connectors | Cable swaps, link lights, NIC status |

**Key insight:** Troubleshooting follows the layers. Start at Layer 1 (is the cable connected?) and work up. This prevents wasting time on complex Layer 7 diagnostics when the cable is simply unplugged.

### TCP/IP Protocol Suite

The practical networking model used in real-world implementations:

- **Network Access (Link):** Ethernet, Wi-Fi, physical connectivity
- **Internet:** IP addressing (IPv4, IPv6), routing between networks
- **Transport:** TCP (reliable, connection-oriented) and UDP (fast, connectionless)
- **Application:** HTTP, DNS, DHCP, SSH, SMTP, and other user-facing protocols

### IP Addressing Fundamentals

- IPv4 addresses: 32-bit, dotted decimal (e.g., 192.168.1.100)
- Subnet masks define network vs. host portions
- Private address ranges: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
- Default gateway: the router that provides access to other networks

### DNS and DHCP

- **DNS:** Translates hostnames to IP addresses. Without DNS, users must know IP addresses directly.
- **DHCP:** Automatically assigns IP configuration (address, mask, gateway, DNS) to devices. Critical in high-density environments.

### Network Devices

- **Switch:** Connects devices within a LAN, operates at Layer 2
- **Router:** Connects different networks, operates at Layer 3
- **Access Point:** Provides wireless connectivity
- **Firewall:** Controls traffic based on rules

---

## OTS Relevance

Every concept in this module maps directly to daily OTS responsibilities:

- **OSI model:** Provides the structured troubleshooting methodology OTS engineers use to isolate problems layer by layer
- **TCP/IP:** The protocol suite running on every device in an Amazon FC; understanding it is non-negotiable for network support
- **IP addressing:** OTS engineers read `ipconfig` output, verify correct addressing, identify APIPA (169.254.x.x) failures, and understand subnet boundaries
- **DNS/DHCP:** Two of the most common service failures that generate support tickets in enterprise environments
- **Network devices:** OTS engineers identify, locate, and verify the status of switches, routers, and access points throughout FC infrastructure

Understanding these foundations enables an OTS engineer to:
1. Identify which layer a problem exists at
2. Use the appropriate diagnostic tool for that layer
3. Communicate findings clearly to network engineering teams
4. Document resolution steps in runbooks for future reference

---

## Commands and Tools to Practice

### Windows

```
ipconfig /all              # View full IP configuration
ipconfig /release          # Release DHCP lease
ipconfig /renew            # Request new DHCP lease
ipconfig /flushdns         # Clear DNS cache
ping [IP or hostname]      # Test reachability
tracert [destination]      # Trace network path
nslookup [hostname]        # Test DNS resolution
arp -a                     # View ARP cache (IP-to-MAC mappings)
netstat -an                # View active connections and listening ports
```

### Linux

```
ip addr show               # View IP configuration
ip link show               # View interface status (UP/DOWN)
ping [IP or hostname]      # Test reachability
traceroute [destination]   # Trace network path
nslookup [hostname]        # Test DNS resolution
dig [hostname]             # Detailed DNS query
sudo dhclient -r           # Release DHCP lease
sudo dhclient              # Request new DHCP lease
ss -tuln                   # View listening ports
```

### Cisco IOS (for reference)

```
show ip interface brief    # View interface status and IP addresses
show running-config        # View current configuration
show mac address-table     # View MAC-to-port mappings
show vlan brief            # View VLAN assignments
ping [destination]         # Test reachability from network device
```

---

## What I Still Need to Learn

- Subnetting calculations (CIDR notation, network/host boundaries, usable addresses)
- How VLANs segment network traffic and why misassignment causes connectivity loss
- The difference between Layer 2 and Layer 3 switching
- How spanning tree protocol prevents loops and can cause port blocking
- IPv6 addressing and its role in modern enterprise networks
- Wireless networking fundamentals (802.11 standards, SSIDs, channels)
- How to read and interpret packet captures (Wireshark basics)

---

## Resume and Interview Connection

This module supports the following interview talking points:

- "I understand the OSI model and use it as a framework for structured troubleshooting, starting at the physical layer and working up."
- "I can verify IP configuration using ipconfig and ip addr, identify DHCP failures by recognizing APIPA addresses, and test connectivity with ping and traceroute."
- "I understand how DNS and DHCP operate as critical network services and can diagnose common failures in both."
- "I am building these skills through structured Coursera coursework aligned with Cisco certification objectives."

**Framing note:** These are skills I am actively developing through coursework, not production experience I am claiming. The appropriate framing is: "I am studying..." or "I am building competency in..." rather than "I have experience with..."

---

## References

- Cisco Network Fundamentals Specialization (Coursera)
- Cisco CCNA 200-301 exam objectives: Section 1.0 (Network Fundamentals)
