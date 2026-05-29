# Device Offline Troubleshooting

## Issue Summary

A network-connected device (switch, access point, printer, scanner, workstation, or infrastructure component) is showing as offline in monitoring systems or is unreachable via ping. The device was previously operational and has stopped responding. In a fulfillment center environment, device availability directly impacts operational uptime, making rapid identification and resolution critical.

## Symptoms

- Monitoring system (Nagios, PRTG, CloudWatch, or similar) shows device as "down" or "unreachable"
- Ping to device IP returns "Request timed out" or "Destination host unreachable"
- Users report inability to use a shared resource (printer, scanner, kiosk)
- Network port shows no link light
- Device management interface is inaccessible
- Dependent services or applications report upstream connectivity failures

## Likely Causes

1. **Power failure:** Device lost power due to outlet issue, power supply failure, or power strip/UPS failure
2. **Network path failure:** Cable disconnected, switch port down, or upstream switch failure
3. **Device hardware failure:** Internal component failure causing device to become unresponsive
4. **Configuration change:** Recent change (firmware update, config push) caused device to become unreachable
5. **IP address change or conflict:** Device IP changed unexpectedly or conflicts with another device
6. **Environmental factors:** Overheating, physical damage, or environmental conditions affecting device operation

## Diagnostic Steps

1. **Verify the alert is legitimate:**
   - Ping the device from multiple sources to rule out monitoring system false positive
   - Check if monitoring system itself is having issues
   - Confirm the correct IP is being monitored

2. **Check connectivity from different paths:**
   - Ping from the same subnet (rules out routing issues)
   - Ping from a different subnet (tests full network path)
   - Traceroute to identify where packets stop: `tracert [device IP]` (Windows) or `traceroute [device IP]` (Linux)

3. **Check the physical location:**
   - Verify power LED is lit on the device
   - Check network port link lights
   - Look for physical damage or disconnected cables
   - Check if the device was accidentally unplugged or moved

4. **Check upstream infrastructure:**
   - Verify the switch port the device connects to is active
   - Check if other devices on the same switch are also affected (indicates switch issue)
   - Verify uplink status on intermediate switches

5. **Check for recent changes:**
   - Were any firmware updates pushed to this device type?
   - Were any network changes made (VLAN, ACL, routing)?
   - Was any physical work done in the area (construction, cleaning, rearrangement)?

6. **Check ARP and MAC tables:**
   - On the connected switch: `show mac address-table` for the device port
   - If MAC is missing, the device is not transmitting (hardware or power issue)
   - If MAC is present but IP unreachable, check for IP/config issues

## Resolution Steps

### Power Failure
1. Verify the power outlet is functioning (test with another device)
2. Check the power cable connection at both ends
3. If using a UPS or power strip, verify it is operational and not overloaded
4. Power cycle the device (unplug, wait 30 seconds, reconnect)
5. If device does not power on, document and escalate for hardware replacement

### Network Path Failure
1. Verify cable is securely connected at device and switch port
2. Replace cable with a known-good cable
3. Try a different switch port (document the port change)
4. If switch port is administratively down, escalate to network team for re-enablement
5. Verify link light returns after cable/port correction

### Device Hardware Failure
1. Attempt power cycle
2. Check for error LEDs or diagnostic indicators on the device
3. If device has a console port, attempt console access for diagnostic information
4. If device does not respond after power cycle, classify as hardware failure
5. Document: device type, location, asset tag, symptoms, and escalate for replacement

### Configuration Change
1. Determine what change was made and when
2. If a rollback is possible and authorized, revert the configuration
3. If firmware update caused the issue, check vendor release notes for known issues
4. Escalate to the team that made the change with findings

### IP Address Change or Conflict
1. Check DHCP lease records for the device MAC address
2. If the device has a new IP, update monitoring and documentation
3. If conflict exists, identify the conflicting device by MAC address
4. Resolve conflict by releasing one address or assigning a static reservation

## Escalation Notes

- **When to escalate:** If device is confirmed powered on but unresponsive (hardware failure), if switch port or infrastructure changes are needed, if multiple devices are offline simultaneously (infrastructure event), or if SLA/uptime impact exceeds threshold
- **Information to provide:** Device type, hostname, IP address, physical location, asset tag, power status, link light status, ping results from multiple sources, steps already attempted, time the device was last seen online, and operational impact assessment
- **Relevant teams:** Network engineering (switch port, routing, infrastructure), facilities (power, cabling, physical access), device vendor support (hardware failure), operations management (impact notification)

## What I Learned

- In fulfillment center environments, device offline events have direct operational impact and require fast triage
- The diagnostic order matters: verify the alert, check power, check physical, check network path, then check configuration
- Maintaining accurate documentation of device locations, IPs, and connections dramatically speeds up troubleshooting
- Understanding the difference between a single-device failure and an infrastructure failure (multiple devices affected) changes the response approach entirely
- Uptime awareness is a core OTS responsibility; knowing SLA implications helps prioritize response

## References

- Cisco Network Fundamentals: Physical and data link layer troubleshooting
- Cisco CCNA 200-301: Network monitoring and management
- Amazon OTS responsibility: Maintaining uptime and availability of FC network infrastructure
