# No Network Connectivity

## Issue Summary

A workstation or endpoint device has completely lost network connectivity. The user cannot access internal resources, browse the internet, or communicate with other devices on the network. This scenario is common in environments with high device density and frequent physical changes, such as fulfillment center operations floors.

## Symptoms

- No internet or intranet access from the affected device
- Browser displays "No Internet" or similar connection error
- Ping to default gateway fails (request timed out or destination unreachable)
- Network icon shows disconnected or limited status
- Applications dependent on network services fail to load
- NIC link light is off or amber (if physical issue)

## Likely Causes

1. **Physical layer failure:** Cable disconnected, damaged, or plugged into wrong port
2. **NIC disabled or failed:** Network interface card is disabled in OS or has a driver issue
3. **DHCP lease failure:** Device failed to obtain an IP address from the DHCP server
4. **Switch port issue:** Port is administratively down, in wrong VLAN, or has a spanning tree problem
5. **IP address conflict:** Another device on the network has the same IP address
6. **Upstream network outage:** Switch, router, or uplink failure affecting multiple devices

## Diagnostic Steps

1. **Physical inspection:** Verify the Ethernet cable is securely connected at both the device and the wall jack or switch port. Check for visible cable damage. Confirm link lights are active on the NIC.

2. **Check NIC status:**
   - Windows: `ncpa.cpl` to view adapter status; confirm adapter is enabled
   - Linux: `ip link show` to verify interface state (UP vs DOWN)

3. **Check IP configuration:**
   - Windows: `ipconfig /all`
   - Linux: `ip addr show`
   - Look for: valid IP address (not 169.254.x.x), subnet mask, default gateway, DNS servers

4. **Ping the default gateway:**
   - `ping [gateway IP]`
   - Success indicates local network connectivity; failure suggests Layer 1-2 issue or gateway problem

5. **Ping a known external IP:**
   - `ping 8.8.8.8`
   - Success with gateway ping but failure here suggests a routing issue upstream

6. **Check for IP conflicts:**
   - Windows: Event Viewer > System logs for conflict warnings
   - `arp -a` to check for duplicate MAC entries

7. **Test with a known-good cable or port:**
   - Swap the Ethernet cable
   - Try a different wall jack or switch port if accessible

## Resolution Steps

### Physical Layer Failure
1. Replace the Ethernet cable with a known-good cable
2. Verify connection at both ends (device and patch panel/switch)
3. Confirm link light returns to solid green
4. Test connectivity with `ping` to default gateway

### NIC Disabled or Driver Issue
1. Enable the network adapter via OS network settings
2. If driver issue suspected: Device Manager > Network Adapters > Update or reinstall driver
3. Restart the network service or reboot the device if needed
4. Verify connectivity returns with `ipconfig /all` and `ping`

### DHCP Lease Failure
1. Release and renew the DHCP lease:
   - Windows: `ipconfig /release` then `ipconfig /renew`
   - Linux: `sudo dhclient -r` then `sudo dhclient`
2. If renewal fails, check DHCP server availability (see dhcp-failure runbook)
3. As a temporary workaround, assign a static IP within the correct subnet

### Switch Port Issue
1. Document the switch and port number for the affected connection
2. Verify with network team whether the port is administratively down
3. If accessible, check port status: `show interface status` on the switch
4. Request port re-enablement or VLAN correction from the network team

### IP Address Conflict
1. Release the current IP: `ipconfig /release`
2. Renew to obtain a new address: `ipconfig /renew`
3. If conflict persists, identify the conflicting device using `arp -a` and MAC address lookup
4. Escalate to network team for DHCP scope review

## Escalation Notes

- **When to escalate:** If multiple devices are affected simultaneously, if the issue persists after cable swap and DHCP renewal, or if switch port changes are needed
- **Information to provide:** Device hostname, MAC address, switch/port identification, IP configuration output, steps already attempted, and whether the issue affects one device or many
- **Relevant teams:** Network engineering (switch port and VLAN issues), desktop support (NIC hardware), facilities (cabling infrastructure)

## What I Learned

- Structured Layer 1 through Layer 3 troubleshooting narrows the problem space efficiently
- Physical layer issues are the most common cause of single-device connectivity loss and should always be checked first
- In high-density environments like fulfillment centers, cable management and port labeling are critical to rapid resolution
- Documenting switch/port information before escalation saves time for network teams

## References

- Cisco Network Fundamentals Specialization: OSI model and troubleshooting methodology
- CompTIA Network+ troubleshooting methodology (bottom-up approach)
