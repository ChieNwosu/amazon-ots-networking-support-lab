# IT Support Engineering Glossary

A beginner-friendly glossary of foundational IT terms, written for context relevant to Amazon IT Support Engineer I (Ops Tech Solutions) responsibilities.

---

## Computing Hardware and Systems

### CPU (Central Processing Unit)

- **Definition:** The processor that executes instructions for every operation a computer performs. Its speed is measured in Gigahertz (GHz).
- **Why it matters in IT support:** When a device is slow or unresponsive, understanding CPU load helps determine whether the issue is a hardware bottleneck or a software/network problem.
- **Amazon FC example:** A handheld scanner's internal processor handles barcode decoding and communication with the warehouse management system. If scans are delayed, CPU performance is one factor to rule out.

### RAM (Random Access Memory)

- **Definition:** Temporary memory that holds data for all currently running programs. Measured in Gigabytes (GB).
- **Why it matters in IT support:** Insufficient RAM causes applications to lag or crash. Knowing how much RAM a device has helps determine if it can handle its assigned workload.
- **Amazon FC example:** A pack station laptop running a browser, label printing software, and inventory tracking simultaneously needs enough RAM to avoid freezing during peak volume.

### Storage (HDD/SSD)

- **Definition:** Persistent memory where data remains after a device powers off. SSDs (Solid State Drives) are faster and more reliable than older HDDs (Hard Disk Drives).
- **Why it matters in IT support:** Storage failures can prevent devices from booting. Knowing the difference between drive types helps diagnose performance issues and plan replacements.
- **Amazon FC example:** A label printer stores its configuration and operating system on internal storage. If the drive fails, the printer cannot initialize after a power cycle.

### Server

- **Definition:** A high-specification computer designed to serve multiple users or applications simultaneously. Servers typically have significantly more CPU cores, RAM, and redundant components than consumer PCs.
- **Why it matters in IT support:** Servers host the services that endpoints depend on. Understanding server architecture helps engineers identify single points of failure.
- **Amazon FC example:** A rack-mounted server in the facility's main distribution frame (MDF) room may host the local print spooler that all pack stations rely on.

### Virtualization

- **Definition:** Using software (a hypervisor) to divide one physical server into multiple isolated virtual machines (VMs), each running its own operating system.
- **Why it matters in IT support:** Virtualization means a single hardware failure can affect multiple services. Engineers need to understand which VMs run on which physical hosts.
- **Amazon FC example:** One physical server might run separate VMs for security camera management, door access controls, and local file storage — all isolated from each other.

---

## Networking and Connectivity

### LAN (Local Area Network)

- **Definition:** A network connecting devices within a limited area such as a building or campus. Devices on a LAN can share resources like printers and file servers.
- **Why it matters in IT support:** Most day-to-day troubleshooting happens within the LAN. Understanding LAN topology helps isolate where a connectivity break occurs.
- **Amazon FC example:** The internal network connecting all scanners, workstations, printers, and robotics units within a single fulfillment center.

### WAN (Wide Area Network)

- **Definition:** A network that connects geographically separated LANs, often spanning cities or countries.
- **Why it matters in IT support:** WAN issues affect communication between sites. If a local device works fine internally but cannot reach a remote service, the problem may be in the WAN link.
- **Amazon FC example:** The high-speed connection linking a fulfillment center to Amazon's cloud-hosted inventory and logistics systems.

### Router

- **Definition:** A device that forwards data packets between different networks by consulting a routing table to determine the best path.
- **Why it matters in IT support:** Routers are the boundary between network segments. If devices can communicate locally but not externally, the router or its configuration is a primary suspect.
- **Amazon FC example:** The edge router that sends traffic from local pack stations out to the internet or into AWS-hosted services.

### Switch

- **Definition:** A device that connects multiple devices within a LAN and forwards data only to the specific port where the destination device is connected.
- **Why it matters in IT support:** Switches provide dedicated bandwidth to each connected device. A failed switch port or misconfigured VLAN can isolate a device from the network.
- **Amazon FC example:** A 48-port switch in a pick zone providing wired connectivity to nearby workstations and printers.

### IP Address

- **Definition:** A unique numerical identifier assigned to every device on a network. IPv4 addresses use a 32-bit dotted-decimal format (e.g., 10.0.1.50). IPv6 uses 128-bit hexadecimal notation.
- **Why it matters in IT support:** Every network communication depends on correct IP addressing. Duplicate IPs, exhausted address pools, or misconfigured addresses cause connectivity failures.
- **Amazon FC example:** Each thermal label printer has a specific IP address so pack stations know exactly where to send print jobs.

### Subnet

- **Definition:** A logical subdivision of a larger IP network, created using subnet masks to separate groups of devices.
- **Why it matters in IT support:** Subnetting isolates traffic for security and performance. Devices on different subnets cannot communicate without a router, which is both a feature and a common source of confusion.
- **Amazon FC example:** Placing all robotics drive units on a dedicated subnet keeps their high-frequency communication separate from office staff network traffic.

### DNS (Domain Name System)

- **Definition:** The system that translates human-readable domain names (like amazon.com) into IP addresses that computers use to route traffic.
- **Why it matters in IT support:** DNS failures are one of the most common causes of "the internet is down" reports. If DNS is broken, devices cannot resolve names even though the network itself is functional.
- **Amazon FC example:** An associate types an internal portal URL into their browser. DNS translates that name into the server's IP address so the page loads.

### DHCP (Dynamic Host Configuration Protocol)

- **Definition:** A service that automatically assigns IP addresses and network configuration (subnet mask, gateway, DNS server) to devices when they connect to the network.
- **Why it matters in IT support:** Without DHCP, every device would need manual IP configuration. DHCP failures leave devices without addresses, making them unreachable.
- **Amazon FC example:** A new handheld scanner automatically receives an IP address the moment it connects to the warehouse Wi-Fi, requiring zero manual setup.

---

## Cloud, Security, and DevOps

### Cloud Computing

- **Definition:** IT infrastructure and services (servers, storage, databases) delivered over the internet on a pay-as-you-go model, eliminating the need to own and maintain physical hardware for every workload.
- **Why it matters in IT support:** Many enterprise applications now run in the cloud. Engineers must understand that "the server" may not be a physical box in the building — it could be an EC2 instance in a remote data center.
- **Amazon FC example:** The software tracking millions of packages runs on AWS cloud infrastructure rather than on servers physically located in each warehouse.

### Database

- **Definition:** An organized collection of data that can be efficiently searched, updated, and retrieved by applications.
- **Why it matters in IT support:** Databases store critical operational data. If a database becomes unreachable or slow, the applications that depend on it fail.
- **Amazon FC example:** A relational database storing the exact bin location of every item in the fulfillment center, queried thousands of times per minute during peak operations.

### Cybersecurity

- **Definition:** The practice of protecting computers, networks, and data from unauthorized access, theft, or damage.
- **Why it matters in IT support:** OTS engineers are the first line of defense for endpoint security. They manage access controls, respond to alerts, and ensure systems remain available.
- **Amazon FC example:** Firewall rules preventing unauthorized external access to the control systems managing the warehouse conveyor network.

### Git

- **Definition:** A version control system that tracks changes to files over time, enabling collaboration and the ability to revert to previous versions.
- **Why it matters in IT support:** Engineering teams use Git to manage scripts, configuration files, and documentation. Being comfortable with Git means you can pull the latest tools and contribute fixes.
- **Amazon FC example:** An OTS engineer downloading the latest version of a station health-check script from a shared internal repository.

### DevOps

- **Definition:** A set of practices combining software development and IT operations to automate and accelerate the delivery of reliable software.
- **Why it matters in IT support:** DevOps practices mean faster, more frequent updates to the tools engineers use. Understanding the pipeline helps engineers know where a broken update came from.
- **Amazon FC example:** An automated system that tests and deploys firmware updates to thousands of handheld scanners across multiple facilities simultaneously.

### SRE (Site Reliability Engineering)

- **Definition:** A discipline that applies software engineering principles to IT operations, focusing on system reliability, uptime, and incident response.
- **Why it matters in IT support:** SRE principles guide how teams measure reliability, respond to outages, and prevent recurrence through root-cause analysis.
- **Amazon FC example:** After a critical server failure during peak shift, an engineer performs root-cause analysis to identify why it failed and implements changes to prevent recurrence.
