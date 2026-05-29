# IT Fundamentals to OTS Role-Alignment Map

This table maps foundational IT concepts to the specific responsibilities of an Amazon IT Support Engineer I (Ops Tech Solutions). Each row connects a learning topic to what I should understand, the OTS responsibility it supports, a realistic troubleshooting scenario, a suggested artifact to build, and where it belongs in this repository.

---

| IT Fundamentals Topic | What I Should Understand | OTS Responsibility Supported | Example Troubleshooting Scenario | Suggested GitHub Artifact | Suggested Repo Folder |
|---|---|---|---|---|---|
| PC Hardware and Peripherals | Function of CPU, RAM, NIC, and storage. Performance metrics (GHz, GB, bits/s). How server hardware differs from consumer PCs. | Supporting PC, endpoint, and peripheral devices in a Fulfillment Center. | A label printer or pack station thin client is lagging or failing to connect to the network. Hardware metrics indicate the NIC is not receiving link. | Hardware Health Checklist | docs/troubleshooting-runbooks/ |
| Switching and VLANs | How switches forward frames within a LAN. How VLANs logically group and isolate devices for security and performance. | Troubleshooting network connectivity issues in warehouse segments. | A wireless scanner in the Pick zone cannot reach a printer in the Pack zone because the devices are on different VLANs without an inter-VLAN route. | FC VLAN Segment Map | courses/cisco-network-fundamentals/notes/ |
| IP Addressing and Subnetting | CIDR notation, subnet masks, calculating usable host ranges, and identifying network vs. host portions of an address. | Maintaining uptime and availability of IT infrastructure. | A /24 subnet designated for warehouse conveyors has exhausted its usable IP addresses, causing new devices to fail DHCP assignment. | CIDR/Subnetting Cheat Sheet | docs/foundational-it/ |
| Routing and Default Gateways | How routers use forwarding tables and default routes (0.0.0.0/0) to send traffic between networks and to the internet. | Performing root-cause analysis on hardware and software failures. | Handheld scanners can reach local servers but cannot access AWS-hosted inventory tools because the default gateway is misconfigured. | Default Route Flowchart | docs/troubleshooting-runbooks/ |
| DNS Resolution | How DNS maps domain names to IP addresses. Using diagnostic tools like nslookup and dig to verify resolution. | Troubleshooting network connectivity issues. | An FC internal portal fails to load by URL but is reachable via direct IP address, indicating a DNS resolution failure. | DNS Resolution Runbook | docs/troubleshooting-runbooks/ |
| DHCP | How devices automatically receive IP configuration when joining a network. Lease times, scopes, and renewal. | Maintaining uptime and availability of IT infrastructure. | A batch of scanners cannot connect after a facility power event because the DHCP server did not restart properly. | DHCP Failure Runbook | docs/troubleshooting-runbooks/ |
| Linux Administration | CLI navigation (cd, ls, pwd), file management (touch, nano), permissions, and basic Bash scripting for automation. | Supporting Linux-based endpoints and administrative tools. | A kiosk station fails to boot because a critical configuration file has incorrect user permissions after an update. | Linux CLI First Response Guide | courses/linux-private-cloud-administration/notes/ |
| AWS Cloud (VPC/EC2) | VPC architecture, public/private subnets, Internet Gateways, route tables, and EC2 instance basics. | Maintaining uptime and availability of IT infrastructure. | A new inventory database instance in a private subnet is unreachable from the management console because no NAT gateway exists for outbound traffic. | VPC Traffic Flow Diagram | docs/foundational-it/ |
| Network Security | Stateful firewalls (Security Groups) vs. stateless firewalls (Network ACLs). SSH key-based access. Port management. | Cybersecurity awareness and endpoint protection. | An engineer needs to allow a new port for a warehouse automation system without exposing the entire subnet to external traffic. | Security Group vs NACL SOP | docs/troubleshooting-runbooks/ |
| Bash Scripting | Writing scripts to automate routine tasks like health checks, backups, and status reporting. | Maintaining uptime and writing/following SOPs and runbooks. | Reducing manual effort by creating a script that checks the status of all thin-client stations at the start of each shift. | Station Health Check Script | courses/linux-private-cloud-administration/notes/ |
| Observability and SRE | Using metrics (CloudWatch), logs (CloudTrail), and alerts to monitor system health. Root-cause analysis methodology. | Communicating technical findings and performing RCA. | Investigating why a critical server's CPU spiked to 90% during peak shift hours by reviewing CloudWatch metrics and correlating with deployment logs. | SRE Metric Monitoring Guide | docs/foundational-it/ |

---

## How to Use This Map

1. **Identify gaps:** If a topic row has no corresponding artifact in the repo yet, it represents a documentation opportunity.
2. **Guide study sessions:** Use the "What I Should Understand" column to focus study time on the most role-relevant aspects of each topic.
3. **Build artifacts incrementally:** Each suggested artifact is a concrete deliverable that demonstrates understanding and can be referenced during interviews or on a resume.
4. **Connect learning to practice:** The troubleshooting scenarios ground abstract concepts in realistic FC situations.
