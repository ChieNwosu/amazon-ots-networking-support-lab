# OSI-Layered Diagnostics Runbook

## Purpose

The OSI (Open Systems Interconnection) model provides a structured, layer-by-layer framework for isolating connectivity and endpoint issues. Rather than guessing at root causes, this approach systematically narrows the problem from physical infrastructure up through application behavior.

This runbook was created as a learning-lab exercise for Amazon IT Support Engineer I (OTS) preparation. It demonstrates how structured diagnostic thinking applies to fulfillment center IT support scenarios.

---

## When to Use This Runbook

Apply OSI-layered diagnostics when:

- A workstation cannot reach an internal site or cloud-hosted service
- A handheld scanner loses wireless connectivity in a specific zone
- A label printer stops responding to print jobs from pack stations
- An endpoint has an IP address but cannot resolve URLs
- A user can reach a service by IP address but not by hostname
- A device communicates locally but cannot reach external or cloud-hosted services
- An application loads but displays errors, timeouts, or certificate warnings

---

## OSI Layer Diagnostic Table

| OSI Layer | What It Covers | FC-Style Failure Example | First Checks | Useful Commands or Tools | Escalation Trigger |
|---|---|---|---|---|---|
| 1 — Physical | Cabling, connectors, power, NICs, wireless signal, link lights | A workstation has no link light on its Ethernet port after being moved to a new desk | Check cable seating at both ends. Verify link lights on NIC and switch port. Confirm power to device. Check for physical damage to RJ45 connector. | Visual inspection, cable tester, check link LEDs | Cable replaced and reseated but no link light returns; switch port may be dead |
| 2 — Data Link | MAC addressing, switch port assignment, VLAN membership, ARP resolution | A scanner connects to Wi-Fi but cannot communicate with any device on the local network | Verify device shows connected status. Check ARP table for MAC entries. Confirm VLAN assignment matches expected segment. | `arp -a`, check Wi-Fi association status, verify switch port VLAN (conceptual) | Device associates but never receives ARP replies; possible VLAN mismatch or switch port misconfiguration |
| 3 — Network | IP addressing, subnet mask, default gateway, routing | A label printer has an IP address but cannot reach the print server on a different subnet | Verify IP address, subnet mask, and default gateway with ipconfig/ifconfig. Ping the default gateway. Ping the destination IP. Run traceroute to identify where packets stop. | `ipconfig /all`, `ip addr`, `ping`, `tracert` / `traceroute` | Device can reach gateway but packets die at a specific hop; routing issue beyond local control |
| 4 — Transport | TCP/UDP ports, connection establishment, firewall rules | A kiosk can ping the inventory server but the web application times out on port 443 | Verify the destination port is listening. Check local firewall rules. Test connectivity to the specific port. | `netstat -an`, `Test-NetConnection -Port`, `telnet` (if available) | Port is open on server but connection is blocked in transit; firewall rule change needed |
| 5 — Session | Session establishment, authentication handshakes, keep-alive | An associate logs into an internal portal but gets disconnected every few minutes | Check if session timeout settings are too aggressive. Verify authentication tokens are being renewed. Look for load balancer session affinity issues. | Browser developer tools (network tab), event logs, session timeout configuration | Sessions drop consistently at the same interval; server-side session or load balancer configuration issue |
| 6 — Presentation | Data encoding, encryption, TLS/SSL certificates, compression | A workstation displays a certificate error when accessing an internal management portal | Check system date/time (expired cert errors often caused by clock skew). Verify the certificate is trusted. Check TLS version compatibility. | Browser certificate viewer, `openssl s_client` (Linux), system clock verification | Certificate is valid and clock is correct but browser still rejects; certificate chain or intermediate CA issue |
| 7 — Application | Application-specific errors, HTTP status codes, DNS resolution, software configuration | An associate reports that the station health dashboard shows "Service Unavailable" | Check the specific error message or HTTP status code. Verify DNS resolution for the application URL. Test with a different browser or clear cache. Confirm the application service is running on the server. | `nslookup`, `dig`, browser developer tools, application logs | Application returns 503; service is down on the server side and needs restart or developer intervention |

---

## Layer-by-Layer Diagnostic Flow

Follow this sequence when troubleshooting an unknown connectivity issue. Start at Layer 1 and work upward — do not skip layers.

### Step 1 — Physical (Layer 1)

- Confirm the device has power and is turned on
- Check that Ethernet cables are fully seated at both the device and wall jack/switch port
- Verify link lights are active on the NIC and corresponding switch port
- For wireless devices, confirm Wi-Fi radio is enabled and signal strength is adequate
- Try a known-good cable or port if link is not established

### Step 2 — Data Link (Layer 2)

- Confirm the device shows a connected network status (not "No network access")
- Run `arp -a` to verify the device can see other MAC addresses on the local segment
- Conceptually verify the device is on the correct VLAN for its function and location

### Step 3 — Network (Layer 3)

- Run `ipconfig /all` (Windows) or `ip addr` (Linux) to verify IP address, subnet mask, and default gateway
- Confirm the IP was assigned by DHCP or is correctly statically configured
- Ping the default gateway — if this fails, the issue is local (Layer 1-2) or gateway is down
- Ping the destination IP — if gateway responds but destination does not, run `tracert` to find where packets stop

### Step 4 — Transport (Layer 4)

- If ping succeeds but the application does not work, test the specific port
- Use `Test-NetConnection -ComputerName [IP] -Port [port]` (Windows) or equivalent
- Check `netstat -an` on the destination to confirm the service is listening
- Review local and network firewall rules for port blocks

### Step 5 — Session (Layer 5)

- If the connection establishes but drops or requires repeated login, investigate session behavior
- Check for session timeout settings, token expiration, or load balancer affinity
- Review event logs for authentication failures or session termination events

### Step 6 — Presentation (Layer 6)

- If the connection works but content displays incorrectly or shows security warnings, check encoding and encryption
- Verify system clock is accurate (clock skew causes certificate validation failures)
- Check TLS version compatibility between client and server
- Verify certificate trust chain

### Step 7 — Application (Layer 7)

- If all lower layers pass, the issue is application-specific
- Check the exact error message, HTTP status code, or application log
- Verify DNS resolution with `nslookup [hostname]`
- Test with a different browser or client to rule out local software issues
- Confirm the application service is running on the server

---

## Example Scenario 1: Label Printer Offline

**Symptom:** A pack station associate reports that print jobs are not reaching the thermal label printer. The printer's display shows "Ready" but no jobs arrive.

**Layer 1 — Physical:**
Check the Ethernet cable connection at the printer and the wall jack. Verify link lights are active on the printer's NIC. Result: Link light is solid green — physical layer is good.

**Layer 2 — Data Link:**
From a nearby workstation on the same segment, run `arp -a` and look for the printer's known MAC address. Result: MAC address appears in the ARP table — Layer 2 connectivity confirmed.

**Layer 3 — Network:**
Ping the printer's IP address from the pack station workstation. Result: Ping fails. Run `ipconfig /all` on the printer's configuration page — the printer has a 169.254.x.x address (APIPA), meaning DHCP assignment failed.

**Root Cause:** The printer lost its DHCP lease and self-assigned a link-local address. It is no longer on the correct subnet.

**Resolution:** Restart the printer's network interface or reboot the printer to trigger a new DHCP request. If DHCP continues to fail, escalate to investigate DHCP server health or scope exhaustion.

---

## Example Scenario 2: Internal Website Works by IP but Not by URL

**Symptom:** An associate reports that typing "station-health.internal.example.com" into the browser returns "This site can't be reached," but entering the server's IP address (10.0.2.15) directly loads the page.

**Layer 1-3 — Physical through Network:**
The fact that the IP address works confirms physical connectivity, Layer 2 switching, and Layer 3 routing are all functional. Packets reach the destination and return.

**Layer 4 — Transport:**
The web page loads on port 443 via IP, so the port is open and the service is listening. Transport layer is good.

**Layer 7 — Application (DNS):**
Run `nslookup station-health.internal.example.com` from the workstation. Result: "Server: Unknown" and "Non-existent domain." The DNS server cannot resolve the hostname.

**Root Cause:** DNS resolution failure. The internal DNS server either does not have a record for this hostname or the workstation is pointed at the wrong DNS server.

**Resolution:** Verify the workstation's configured DNS server with `ipconfig /all`. If the DNS server is correct, escalate to the team managing DNS records to add or fix the A record.

---

## Example Scenario 3: Scanner Drops Connection in One Area

**Symptom:** Handheld scanners in the east wing of the facility frequently disconnect from the warehouse management system. Scanners in other areas work fine.

**Layer 1 — Physical:**
Check wireless signal strength in the east wing. Walk the area and note signal bars on the scanner. Result: Signal drops to one bar or "No signal" near the far end of the zone. The nearest wireless access point (WAP) may be down or obstructed.

**Verification:** Visually inspect the WAP serving that zone. Result: The access point's power LED is off — it has lost power.

**Root Cause:** The wireless access point covering the east wing has no power, creating a dead zone. Scanners intermittently connect to a distant AP with weak signal, causing drops.

**Resolution:** Check the PoE (Power over Ethernet) connection to the access point. Verify the cable and the switch port providing power. If the port is dead, move the AP to a working port and escalate for switch port repair.

**If the AP had power:** Move to Layer 2 — verify the AP is associated with the correct SSID and VLAN. Check for channel interference from nearby APs or non-Wi-Fi devices.

---

## What I Learned

Building this runbook reinforced several key concepts:

- **Structured troubleshooting prevents wasted time.** Starting at Layer 1 and working up ensures I do not chase application-level theories when the problem is a loose cable. Each layer builds on the one below it.

- **OSI layers map directly to OTS daily work.** Most FC support tickets involve Layers 1-3 (physical connectivity, DHCP/IP issues, routing) and Layer 7 (DNS, application errors). Knowing which layer to investigate first accelerates resolution.

- **Clear escalation points protect uptime.** Not every issue is solvable at the endpoint level. Recognizing when a problem requires network engineering, server administration, or application development intervention — and communicating that clearly — is part of the OTS role.

- **Documentation supports the whole team.** A runbook like this means the next engineer who encounters a similar issue does not start from scratch. This aligns with OTS expectations around SOP authoring and knowledge sharing.

---

## Resume / Interview Connection

- **Structured troubleshooting methodology:** Demonstrates ability to systematically isolate root causes using the OSI model rather than trial-and-error, a core competency for IT support engineering roles.
- **Networking fundamentals applied to real scenarios:** Shows understanding of how physical, network, transport, and application layers interact in enterprise environments with high device density.
- **Technical communication and documentation:** Illustrates the ability to write clear, actionable runbooks that other engineers can follow — directly aligned with OTS expectations for SOP authoring and incident documentation.
