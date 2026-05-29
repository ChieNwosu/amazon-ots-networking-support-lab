# FreeCodeCamp IT Fundamentals — Summary and OTS Relevance

## Overview

FreeCodeCamp's IT Fundamentals course is a supplemental, self-paced learning resource I used to strengthen foundational IT knowledge. It is not a formal certification or accredited program. I completed it as additional preparation alongside my Coursera specializations to fill gaps in hardware, networking, cloud, and security concepts relevant to an Amazon IT Support Engineer I (OTS) role.

This document summarizes the key learning areas and explains why each matters for OTS readiness.

---

## Learning Areas

### 1. Hardware and PC Support

Understanding how CPUs, RAM, storage drives, NICs, and peripheral devices function together. Knowing performance metrics (GHz, GB, bits/s) and how server hardware differs from consumer PCs.

**Why it matters for OTS:** IT Support Engineers troubleshoot endpoint devices daily — thin clients, label printers, handheld scanners, and rack-mounted servers. Recognizing whether a device is underperforming due to hardware limitations versus a network issue is a fundamental diagnostic skill.

---

### 2. Networking

Differentiating between LAN, WAN, and WLAN environments. Understanding how switches forward traffic within a network segment and how routers move packets between networks. Recognizing the role of VLANs in traffic segmentation.

**Why it matters for OTS:** Fulfillment Centers are large-scale LANs with thousands of connected devices. OTS engineers must understand how network infrastructure is organized to isolate connectivity problems to the correct segment, switch, or access point.

---

### 3. DNS, DHCP, and TCP/IP

Understanding IP addressing (IPv4/IPv6), subnet masks, CIDR notation, and how DNS translates domain names to IP addresses. Knowing how DHCP automatically assigns network configuration to devices. Differentiating TCP (reliable, connection-oriented) from UDP (fast, connectionless).

**Why it matters for OTS:** DNS and DHCP failures are among the most common support tickets in high-density device environments. When a scanner cannot reach the warehouse management system, the troubleshooting path often starts with verifying IP assignment (DHCP) and name resolution (DNS).

---

### 4. Linux

Navigating the filesystem via CLI (ls, cd, pwd), managing files (touch, mkdir, nano), understanding user permissions and root access, and writing basic Bash scripts for automation.

**Why it matters for OTS:** Many Amazon infrastructure tools and endpoints run Linux. OTS engineers use the command line to check service status, review logs, verify configurations, and run diagnostic scripts on headless devices.

---

### 5. AWS and Cloud Concepts

Understanding Regions, Availability Zones, VPCs, public/private subnets, Internet Gateways, EC2 instances, and the pay-as-you-go model.

**Why it matters for OTS:** Amazon's warehouse systems rely on cloud-hosted services. Understanding VPC architecture helps OTS engineers grasp why a local device might not reach a cloud-hosted application — whether the issue is a route table misconfiguration, a missing gateway, or a security group rule.

---

### 6. Security

Implementing firewalls at different levels: Security Groups (stateful, instance-level) and Network ACLs (stateless, subnet-level). Using SSH key pairs for secure remote access. Understanding HTTPS and encryption basics.

**Why it matters for OTS:** OTS engineers manage access controls and must understand why certain traffic is allowed or blocked. When a new automation system needs a port opened, the engineer must know how to modify rules without exposing the broader network.

---

### 7. Git and Documentation

Using Git for version control, maintaining change history, and collaborating through repositories. Creating architecture diagrams to document infrastructure.

**Why it matters for OTS:** OTS teams use version-controlled scripts and runbooks. Being comfortable with Git means an engineer can pull the latest diagnostic tools, contribute fixes, and maintain documentation that the whole team relies on.

---

### 8. DevOps and SRE

Understanding monitoring (real-time metrics), alerting (threshold-based notifications), and logging (event archives). Knowing the difference between DevOps (speed of delivery) and SRE (reliability and uptime). Awareness of Infrastructure as Code concepts.

**Why it matters for OTS:** OTS engineers perform root-cause analysis when systems fail. Understanding how monitoring and alerting work helps engineers respond to incidents faster and contribute meaningful data to post-incident reviews.

---

## How This Fits Into My Preparation

This course provided broad foundational coverage that complements the deeper, certification-aligned Coursera specializations I am completing (Cisco networking, Linux administration, cybersecurity operations). It helped me build a mental model of how hardware, networking, cloud, and security concepts connect before diving into protocol-level detail.

It is supplemental learning — not a credential — and I treat it as one input among several in my OTS preparation.
