# DNS Resolution Diagnostics Runbook

## Purpose

DNS (Domain Name System) translates hostnames into IP addresses. When DNS fails, users experience what looks like a network outage or application failure — but the underlying connectivity is often fine. The device can reach servers by IP address; it simply cannot resolve names.

In a fulfillment center environment with thousands of endpoints relying on internal portals, cloud-hosted tools, and print services, DNS failures generate high-volume support tickets quickly. An IT support engineer who can distinguish a DNS issue from a true network outage resolves problems faster and escalates with precision.

This runbook was created as a learning-lab exercise for Amazon IT Support Engineer I (Ops Tech Solutions) preparation.

---

## When to Use This Runbook

Use this diagnostic approach when:

- A workstation can reach a server by IP address but not by hostname
- An internal web tool does not load by URL but the server is confirmed online
- Only one endpoint has DNS issues while others on the same subnet work fine
- Multiple endpoints in the same area cannot resolve any hostnames
- A browser displays "This site can't be reached" or "DNS_PROBE_FINISHED_NXDOMAIN"
- Ping by IP succeeds but ping by hostname fails or returns "could not find host"
- A device recently changed subnets or received a new DHCP lease

---

## Key Concepts

### DNS (Domain Name System)
A distributed system that maps human-readable hostnames (like inventory-portal.internal) to numerical IP addresses (like 10.0.4.20). Without DNS, users would need to memorize IP addresses for every service.

### Hostname and Domain Name
A hostname identifies a specific device or service (e.g., print-server-01). A domain name is the broader namespace (e.g., warehouse.internal). Together they form a fully qualified domain name (FQDN): print-server-01.warehouse.internal.

### DNS Resolver
The software component on a device that sends DNS queries to a DNS server. When you type a URL, the resolver asks "what IP address does this name point to?"

### DNS Cache
A local store of recent DNS lookups. The device remembers answers for a period of time (TTL — time to live) to avoid repeated queries. Stale cache entries can cause failures if a server's IP has changed.

### Hosts File
A local text file that maps hostnames to IP addresses before DNS is consulted. Entries here override DNS. A misconfigured hosts file can silently redirect or block name resolution.

### DHCP-Provided DNS Server
When a device receives its IP configuration via DHCP, it also receives the address of one or more DNS servers to use. If DHCP provides an incorrect or unreachable DNS server address, the device cannot resolve names.

### Internal vs. Public DNS
Internal DNS resolves private hostnames (like intranet portals) that do not exist on the public internet. Public DNS (like 8.8.8.8) resolves internet domains. A device configured with only public DNS cannot reach internal resources by name.

### nslookup and dig
Command-line tools that query DNS servers directly. They show whether a name resolves, which server answered, and what IP was returned — essential for isolating whether the problem is the DNS server, the network path to it, or the client configuration.

---

## Diagnostic Flow

Follow this sequence to isolate DNS issues systematically.

### Step 1 — Confirm Physical and Network Connectivity
- Verify the device has an active network connection (link light, Wi-Fi association)
- Confirm the device is not in a disconnected or limited-connectivity state

### Step 2 — Confirm IP Configuration
- Run `ipconfig /all` (Windows) or `ip addr` and `cat /etc/resolv.conf` (Linux)
- Verify the device has a valid IP address (not 169.254.x.x / APIPA)
- Note the assigned DNS server address(es)

### Step 3 — Test Reachability by IP Address
- Ping the destination server by its known IP address
- If ping by IP fails, the issue is network connectivity — not DNS. Refer to the network connectivity runbook.
- If ping by IP succeeds, proceed to DNS testing.

### Step 4 — Test Hostname Resolution
- Run `nslookup <hostname>` to query DNS directly
- If nslookup returns the correct IP, DNS is working — the issue may be browser cache, proxy, or application-level
- If nslookup fails (timeout or NXDOMAIN), DNS resolution is broken for this device

### Step 5 — Check DNS Server Assignment
- Verify the DNS server IP shown in `ipconfig /all` or `/etc/resolv.conf`
- Ping the DNS server IP to confirm it is reachable
- If the DNS server is unreachable, the issue is either the DNS server being down or a routing problem between the device and the DNS server

### Step 6 — Flush DNS Cache
- Windows: `ipconfig /flushdns`
- Linux: `systemd-resolve --flush-caches` or `resolvectl flush-caches` (systemd-based) or restart nscd/dnsmasq if applicable
- Retry the hostname resolution after flushing
- Stale cache entries are a common cause of "it worked yesterday" failures

### Step 7 — Compare One Endpoint vs. Multiple Endpoints
- If only one device is affected: likely a local configuration issue (wrong DNS server, stale cache, hosts file override)
- If multiple devices on the same subnet are affected: likely a DHCP-supplied DNS issue or the DNS server itself is down
- Test from a second device on the same subnet to confirm scope

### Step 8 — Escalate if DNS-Server-Side or Network-Wide
- If the DNS server is unreachable from multiple devices, escalate to network engineering
- If the DNS server is reachable but returning incorrect answers, escalate to the DNS/infrastructure team
- Document what was tested, what worked, and what failed before escalating

---

## Useful Commands

### Windows

| Command | Purpose |
|---------|---------|
| `ipconfig /all` | Show IP address, subnet, gateway, and DNS server assignment |
| `ipconfig /flushdns` | Clear the local DNS cache |
| `nslookup hostname` | Query DNS for a specific hostname |
| `nslookup hostname 8.8.8.8` | Query a specific DNS server (useful for comparing internal vs. public) |
| `ping hostname` | Test if hostname resolves and is reachable |
| `ping IP-address` | Test raw IP reachability (bypasses DNS) |
| `tracert hostname` | Trace the network path to a hostname |
| `netsh interface ip show dns` | Show DNS configuration per interface |

### Linux

| Command | Purpose |
|---------|---------|
| `ip addr` | Show IP address and interface status |
| `cat /etc/resolv.conf` | Show configured DNS server(s) |
| `dig hostname` | Query DNS with detailed response information |
| `dig hostname @8.8.8.8` | Query a specific DNS server |
| `nslookup hostname` | Basic DNS lookup |
| `ping hostname` | Test hostname resolution and reachability |
| `ping IP-address` | Test raw IP reachability |
| `traceroute hostname` | Trace the network path |
| `systemd-resolve --flush-caches` | Flush local DNS cache (systemd) |
| `resolvectl status` | Show current DNS resolver configuration |

---

## Example Scenario 1: IP Works, URL Fails

**Symptom:** An associate reports that the inventory portal will not load. The browser shows "This site can't be reached." Other associates on different floors can access it fine.

**Step 1 — Connectivity:** Device has a valid network connection. Other websites load normally.

**Step 3 — Ping by IP:** Ping the portal server's known IP (10.0.4.20) — success. The network path is fine.

**Step 4 — nslookup:**
```
> nslookup inventory-portal.internal
Server: 10.0.1.1
DNS request timed out.
```
The DNS server is not responding.

**Step 5 — Ping DNS server:** Ping 10.0.1.1 — timeout. The configured DNS server is unreachable from this device.

**Step 2 — Check configuration:** `ipconfig /all` shows DNS server 10.0.1.1. Other working devices on a different subnet show DNS server 10.0.2.1.

**Root cause:** This device is on a subnet whose DHCP scope provides a DNS server address (10.0.1.1) that is no longer reachable — possibly decommissioned or moved. The DHCP scope needs updating.

**Escalation:** Network/DHCP team to correct the DNS server address in the DHCP scope for this subnet.

---

## Example Scenario 2: One Workstation Has DNS Issues

**Symptom:** A single pack station workstation cannot load any internal sites by name. The workstation next to it works fine. Both are on the same subnet.

**Step 3 — Ping by IP:** Ping the internal portal by IP — success. Network is fine.

**Step 4 — nslookup:** Returns NXDOMAIN for all internal hostnames.

**Step 5 — Check DNS server:** `ipconfig /all` shows DNS server 8.8.8.8 (Google public DNS). The neighboring workstation shows 10.0.2.1 (internal DNS).

**Investigation:** This workstation has a manually configured DNS server instead of using DHCP-assigned settings. Someone previously set it to use public DNS, which cannot resolve internal hostnames.

**Step 6 — Fix:** Change the network adapter settings back to "Obtain DNS server address automatically" and run `ipconfig /flushdns`.

**Root cause:** Manual DNS override pointing to a public resolver that has no knowledge of internal domain names.

**Resolution time:** Under 5 minutes once the configuration mismatch was identified.

---

## Example Scenario 3: Multiple Devices Cannot Resolve Names

**Symptom:** Five workstations and several scanners in the east wing all report that internal tools are unreachable. External sites (google.com) also fail to load by name. Devices in other areas work normally.

**Step 3 — Ping by IP:** Ping an internal server by IP from an affected device — success. Ping 8.8.8.8 — success. Network connectivity is intact.

**Step 4 — nslookup:** All affected devices timeout when querying DNS.

**Step 5 — DNS server check:** All affected devices show DNS server 10.0.3.5 (assigned via DHCP). Ping 10.0.3.5 from affected devices — timeout.

**Step 7 — Scope check:** Devices in other areas use DNS server 10.0.2.1, which responds normally. The east wing subnet's DHCP scope assigns a different DNS server.

**Investigation:** DNS server 10.0.3.5 is down or unreachable from this subnet. This affects all devices that received this DNS assignment.

**Escalation:** Infrastructure team to investigate DNS server 10.0.3.5 availability. Immediate workaround: if authorized, temporarily configure affected devices to use 10.0.2.1 while the primary is restored.

**Root cause:** DNS server failure affecting all endpoints in a specific DHCP scope.

---

## Escalation Notes

Escalate to network engineering or infrastructure support when:

- The DNS server itself is confirmed unreachable from multiple devices (not a single-device config issue)
- The DNS server is reachable but returning incorrect or stale records
- DHCP is distributing an incorrect DNS server address to an entire subnet
- The issue affects a zone or building-wide scope rather than individual endpoints
- You have confirmed the problem is not local cache, hosts file, or manual configuration

When escalating, include:
- Which hostnames fail and which (if any) succeed
- The DNS server IP assigned to affected devices
- Whether the DNS server responds to ping
- Whether the issue is isolated to one device or affects multiple
- What you have already tested and ruled out

---

## What I Learned

Building this runbook reinforced how frequently DNS issues masquerade as network or application failures. The key insight is that if a device can reach an IP address but not a hostname, the network is working — the translation layer is broken.

This connects directly to OTS responsibilities:
- **Uptime:** DNS failures can take down access to every internal tool simultaneously, making rapid diagnosis critical
- **Root-cause analysis:** Distinguishing "DNS server is down" from "one device has wrong DNS config" determines whether the fix is local (5 minutes) or requires infrastructure team involvement
- **Technical communication:** Escalating with specific findings (which DNS server, what it returns, how many devices affected) gives the next team actionable information instead of a vague "DNS is broken"

---

## Resume / Interview Connection

- **Network troubleshooting:** Demonstrates ability to isolate DNS failures from network connectivity issues using systematic diagnostic steps and standard tools (nslookup, dig, ipconfig)
- **Endpoint diagnostics and DNS/DHCP understanding:** Shows knowledge of how DHCP-assigned DNS configuration affects endpoint behavior and how misconfigurations create localized or widespread resolution failures
- **Structured escalation:** Documents clear criteria for when to resolve locally versus escalate, and what information to provide when handing off — reflecting OTS expectations for professional incident communication
