# Weekly Learning Log

## Week Of
June 2, 2025

## Focus Area
Networking Foundations and Cloud Infrastructure

## Goals This Week

- Transition from hardware basics to networking and cloud architecture
- Understand the OSI model as a troubleshooting framework
- Learn IP addressing, subnetting, and CIDR notation
- Study routing and switching fundamentals
- Begin AWS VPC architecture concepts (subnets, gateways, security)

---

## What I Studied

| Topic | Key Focus | Source |
|-------|-----------|--------|
| OSI Model | 7-layer framework for network communication and troubleshooting | FreeCodeCamp IT Fundamentals (supplemental) |
| IP Addressing | IPv4 structure, dotted-decimal notation, network vs. host bits | FreeCodeCamp IT Fundamentals (supplemental) |
| Subnetting | CIDR notation, subnet masks, dividing address blocks | FreeCodeCamp IT Fundamentals (supplemental) |
| Routing and Switching | Switch forwarding within LANs, router forwarding between networks | FreeCodeCamp IT Fundamentals (supplemental) |
| DNS | Name-to-IP translation, resolution process, diagnostic tools | FreeCodeCamp IT Fundamentals (supplemental) |
| AWS VPC Basics | Regions, Availability Zones, VPC creation, subnet types | FreeCodeCamp IT Fundamentals (supplemental) |
| Cloud Security | Security Groups, Network ACLs, SSH key pairs | FreeCodeCamp IT Fundamentals (supplemental) |

---

## Key Concepts Learned

### OSI Model
A 7-layer framework that standardizes how data travels across a network. Each layer has a specific function, from physical cabling (Layer 1) to application protocols (Layer 7). The model is essential for structured troubleshooting — it helps isolate whether a problem is physical, logical, or application-level.

### IP Addressing and Subnetting
IPv4 addresses are 32-bit numbers written in dotted-decimal format. CIDR notation (e.g., /24) indicates how many bits define the network portion versus the host portion. Subnetting divides a large address block into smaller segments, improving security and conserving addresses.

### Routing and Switching
Switches operate within a single network segment (LAN), forwarding frames based on MAC addresses. Routers operate between networks, forwarding packets based on IP addresses using routing tables. The default gateway (often 0.0.0.0/0) tells a device where to send traffic destined for networks it does not know about.

### DNS
The Domain Name System translates human-readable names into IP addresses. When DNS fails, devices cannot resolve names even though the underlying network is functional. Tools like nslookup and dig help verify whether DNS resolution is working.

### AWS VPC Architecture
A Virtual Private Cloud (VPC) is an isolated virtual network within AWS. Key components:
- **Public subnets** have a route to an Internet Gateway (IGW), making resources internet-accessible
- **Private subnets** have no direct internet route, keeping resources hidden
- **Route tables** define where traffic is directed within the VPC
- **Internet Gateways** provide the bridge between VPC resources and the public internet

### Cloud Security Controls
- **Security Groups** are stateful firewalls at the instance level — if inbound traffic is allowed, the response is automatically permitted
- **Network ACLs** are stateless firewalls at the subnet level — both inbound and outbound rules must be explicitly configured
- Both work together to create layered defense

---

## Connection to Amazon FC Operations

- **Device connectivity:** Fulfillment Centers function as massive LANs where thousands of scanners, printers, and robotics units must each have a unique, routable address to receive instructions
- **Operational redundancy:** Distributing applications across multiple Availability Zones ensures that if one data center segment fails, package flow continues
- **Traffic segmentation:** Subnetting isolates sensitive corporate data from guest Wi-Fi and keeps high-frequency robotics traffic separate from office workloads
- **Uptime dependency:** Every minute of network downtime in an FC directly impacts package throughput. The infrastructure concepts I studied this week are the foundation of that availability.

---

## Connection to OTS Responsibilities

- **Structured troubleshooting:** The OSI model gives OTS engineers a systematic approach — determine if the issue is physical (Layer 1, broken cable), data-link (Layer 2, switch port), network (Layer 3, routing), or higher
- **Security management:** Configuring Security Groups is a routine OTS task — ensuring only authorized traffic (like SSH on port 22) reaches warehouse endpoints
- **Root-cause analysis:** Understanding DNS helps diagnose the common scenario where an internal portal loads by IP but not by name, pointing directly to a DNS issue rather than a network outage
- **Infrastructure awareness:** Knowing how VPCs, subnets, and gateways work helps OTS engineers understand why a local device might not reach a cloud-hosted service

---

## What I Still Need to Practice

- **Manual subnetting:** I can use online calculators, but I need to become faster at manually calculating usable host ranges and broadcast addresses for non-standard CIDR blocks like /26 or /28
- **Stateless firewall logic:** Network ACLs require explicit rules for both directions. I need more practice remembering to configure the return path for response traffic
- **Routing table interpretation:** Reading and understanding route table entries, especially distinguishing between local routes and internet-bound routes in a VPC

---

## Next GitHub Artifacts to Create

1. `docs/troubleshooting-runbooks/osi-layered-diagnostics.md` — A guide mapping OSI layers to specific FC hardware failures (Layer 1 = damaged RJ45, Layer 3 = routing misconfiguration)
2. `courses/cisco-network-fundamentals/notes/subnet-design-lab.md` — A documented exercise dividing a 10.0.0.0/16 block into functional FC subnets
3. `assets/images/fc-vpc-architecture.png` — A professional architecture diagram showing a multi-AZ VPC design with public/private tiers

---

## Key Takeaways

- The OSI model is not just theory — it is a practical troubleshooting methodology that OTS engineers use daily
- Subnetting is the foundation of network organization in large facilities. Without it, thousands of devices would create unmanageable broadcast domains
- Cloud security operates in layers (Security Groups + NACLs), and understanding both is necessary for OTS work
- DNS and DHCP failures account for a large percentage of "network down" reports, making them high-priority knowledge areas

---

## Hours Invested

- **Total this week:** 8 hours
- **Running total:** 18 hours
