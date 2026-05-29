# DHCP Failure

## Issue Summary

A device has failed to obtain an IP address from the DHCP server, resulting in an Automatic Private IP Addressing (APIPA) address (169.254.x.x) or no IP configuration at all. The device has physical connectivity but cannot communicate with network resources because it lacks a valid IP address, subnet mask, default gateway, and DNS server configuration.

## Symptoms

- Device shows an IP address in the 169.254.x.x range (APIPA)
- `ipconfig /all` shows "Autoconfiguration IPv4 Address" instead of a DHCP-assigned address
- Limited or no network connectivity despite active link light
- Applications report connection timeouts
- Other devices on the same subnet may be working normally (single-device failure) or multiple devices may be affected (server/scope issue)
- DHCP lease shows "N/A" or expired timestamp

## Likely Causes

1. **DHCP server unreachable:** Server is down, overloaded, or network path to server is broken
2. **DHCP scope exhausted:** All available addresses in the scope have been leased
3. **DHCP relay agent failure:** In routed environments, the relay agent (ip helper-address) is not forwarding DHCP requests to the server
4. **NIC or driver issue:** The network adapter is not sending DHCP Discover packets correctly
5. **VLAN misconfiguration:** Device is on a VLAN that does not have a DHCP scope or cannot reach the DHCP server
6. **Rogue DHCP server:** An unauthorized DHCP server is responding with invalid configuration

## Diagnostic Steps

1. **Verify the current IP configuration:**
   - Windows: `ipconfig /all`
   - Linux: `ip addr show` and `cat /etc/resolv.conf`
   - Confirm the address is 169.254.x.x or blank

2. **Attempt DHCP renewal:**
   - Windows: `ipconfig /release` then `ipconfig /renew`
   - Linux: `sudo dhclient -r` then `sudo dhclient -v` (verbose shows DHCP exchange)
   - Note any error messages during renewal

3. **Check if other devices on the same switch/VLAN are affected:**
   - If only one device: likely a NIC, cable, or port issue
   - If multiple devices: likely a DHCP server or scope issue

4. **Verify physical connectivity:**
   - Ping another device using a static IP (if temporarily assigned)
   - Confirm link light is active and cable is good

5. **Check DHCP server status (if accessible):**
   - Verify the DHCP service is running
   - Check scope utilization (percentage of addresses in use)
   - Review DHCP server logs for errors or denied requests

6. **Capture DHCP traffic (advanced):**
   - Windows: `netsh trace start capture=yes`
   - Linux: `sudo tcpdump -i eth0 port 67 or port 68`
   - Look for Discover packets leaving and Offer packets returning

## Resolution Steps

### DHCP Server Unreachable
1. Verify DHCP server is powered on and service is running
2. Ping the DHCP server from a known-good device to confirm reachability
3. Restart the DHCP service if it has stopped
4. Check network path between the client VLAN and the DHCP server

### DHCP Scope Exhausted
1. Review scope utilization on the DHCP server
2. Identify and remove stale leases (devices no longer on the network)
3. Reduce lease duration to free addresses faster
4. Expand the scope if the subnet allows additional addresses
5. Renew the affected device: `ipconfig /release` and `ipconfig /renew`

### DHCP Relay Agent Failure
1. Verify the `ip helper-address` configuration on the local router/Layer 3 switch
2. Confirm the relay agent can reach the DHCP server
3. Check that the relay agent is forwarding on the correct VLAN interface
4. Test by temporarily placing the DHCP server on the local subnet (if feasible)

### NIC or Driver Issue
1. Disable and re-enable the network adapter
2. Update or reinstall the NIC driver
3. Test with a USB network adapter to rule out hardware failure
4. Verify the adapter is set to obtain an IP address automatically

### VLAN Misconfiguration
1. Verify the switch port VLAN assignment matches the expected DHCP scope
2. Confirm the VLAN has a DHCP scope configured or a relay agent pointing to the correct server
3. Request VLAN correction from the network team if needed

## Escalation Notes

- **When to escalate:** If DHCP renewal fails after multiple attempts, if scope exhaustion is confirmed, if multiple devices are affected, or if relay agent configuration changes are needed
- **Information to provide:** Affected device MAC address, switch/port/VLAN, output of `ipconfig /all`, number of devices affected, timestamp of when the issue started, and results of renewal attempts
- **Relevant teams:** Network engineering (DHCP server, relay agent, VLAN), systems administration (DHCP server health), desktop support (NIC issues)

## What I Learned

- DHCP is a critical dependency for device connectivity in enterprise environments; a single DHCP failure can cascade to many devices
- Understanding the four-step DHCP process (Discover, Offer, Request, Acknowledge) helps identify where the failure occurs
- In large environments like fulfillment centers, DHCP scope planning must account for device density and lease duration
- Relay agents are essential in routed environments and represent a common failure point that is not immediately obvious from the client side

## References

- Cisco Network Fundamentals: DHCP protocol operation
- Cisco CCNA 200-301: IP services (DHCP configuration and troubleshooting)
