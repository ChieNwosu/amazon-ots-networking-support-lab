# DNS Resolution Issue

## Issue Summary

A device can reach network resources by IP address but cannot resolve hostnames to IP addresses. Users experience failures when accessing websites, internal applications, or network shares by name, while direct IP access works normally. This indicates a DNS (Domain Name System) resolution failure rather than a general connectivity problem.

## Symptoms

- Web browsers display "DNS_PROBE_FINISHED_NXDOMAIN" or "Server not found" errors
- Ping by hostname fails, but ping by IP address succeeds (e.g., `ping 8.8.8.8` works, `ping google.com` fails)
- `nslookup` returns "DNS request timed out" or "Server failed"
- Internal applications that rely on hostname resolution fail to connect
- Email clients and collaboration tools report server connection errors
- Users report "internet is down" but the issue is specifically name resolution

## Likely Causes

1. **DNS server unreachable:** Configured DNS server is down or network path is broken
2. **Incorrect DNS configuration:** Wrong DNS server addresses configured on the device
3. **DNS cache corruption:** Local DNS cache contains stale or incorrect entries
4. **DNS server overloaded:** Server is responding slowly or dropping queries under load
5. **Firewall blocking DNS:** UDP port 53 (or TCP 53) traffic is being blocked between client and DNS server
6. **DHCP providing wrong DNS:** DHCP scope is configured with incorrect DNS server addresses

## Diagnostic Steps

1. **Verify DNS configuration:**
   - Windows: `ipconfig /all` (check "DNS Servers" field)
   - Linux: `cat /etc/resolv.conf`
   - Confirm DNS server addresses are correct and expected

2. **Test DNS resolution explicitly:**
   - `nslookup google.com` (uses configured DNS server)
   - `nslookup google.com 8.8.8.8` (uses Google DNS directly)
   - If the second command works but the first does not, the configured DNS server is the problem

3. **Test connectivity to the DNS server:**
   - `ping [DNS server IP]`
   - If ping fails, it may be a routing issue rather than a DNS-specific issue

4. **Flush local DNS cache:**
   - Windows: `ipconfig /flushdns`
   - Linux: `sudo systemd-resolve --flush-caches` or restart the resolver service
   - Re-test name resolution after flushing

5. **Test with alternative DNS:**
   - Temporarily set DNS to 8.8.8.8 (Google) or 1.1.1.1 (Cloudflare)
   - If resolution works with alternative DNS, the issue is with the primary DNS server

6. **Check if the issue is widespread:**
   - Test DNS from another device on the same subnet
   - If multiple devices are affected, the DNS server itself is likely the issue

## Resolution Steps

### DNS Server Unreachable
1. Verify the DNS server is running and accessible from the network
2. Check routing between the client subnet and the DNS server
3. Restart the DNS service if it has stopped
4. If the primary DNS is down, configure the device to use the secondary DNS server temporarily

### Incorrect DNS Configuration
1. Correct the DNS server addresses in network adapter settings
2. If DHCP-assigned, verify the DHCP scope is providing correct DNS server addresses
3. Renew DHCP lease after scope correction: `ipconfig /release` and `ipconfig /renew`
4. Verify with `ipconfig /all` that correct DNS is now configured

### DNS Cache Corruption
1. Flush the DNS cache:
   - Windows: `ipconfig /flushdns`
   - Linux: `sudo systemd-resolve --flush-caches`
2. Verify resolution: `nslookup [target hostname]`
3. If issue persists, check the DNS server for incorrect zone entries

### DNS Server Overloaded
1. Check DNS server performance metrics (CPU, memory, query rate)
2. Identify any query floods or unusual patterns
3. Consider adding DNS server capacity or implementing caching resolvers
4. Verify DNS query timeout settings on the client

### Firewall Blocking DNS
1. Verify firewall rules allow UDP and TCP port 53 outbound to DNS servers
2. Check for recently changed firewall policies
3. Test with: `nslookup google.com [DNS IP]` to confirm blocked vs. server-down
4. Request firewall rule correction from the security team if needed

## Escalation Notes

- **When to escalate:** If DNS server is confirmed down, if DHCP scope needs DNS configuration changes, if firewall rules need modification, or if the issue affects multiple subnets
- **Information to provide:** DNS server IP addresses (configured and expected), output of `nslookup` commands showing failures, whether issue is single-device or widespread, and whether alternative DNS (8.8.8.8) resolves the issue
- **Relevant teams:** Network engineering (DNS server management), systems administration (DNS service health), security team (firewall rules)

## What I Learned

- DNS failures often appear as "no internet" to users, but the diagnostic approach differs significantly from a true connectivity loss
- Testing with an alternative DNS server (8.8.8.8) is a fast way to isolate whether the problem is the DNS server or the network path
- In enterprise environments like Amazon FCs, internal DNS is critical for accessing operational applications, making DNS issues high-priority
- Flushing DNS cache is a low-risk first step that resolves a surprising number of DNS-related complaints

## References

- Cisco Network Fundamentals: DNS protocol and name resolution
- Cisco CCNA 200-301: IP services (DNS)
