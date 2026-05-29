# Workstation and Endpoint Support

## Issue Summary

A workstation or endpoint device is experiencing performance degradation, application failures, or operational issues that are not network-related. These issues affect individual user productivity and may require local diagnostics, software troubleshooting, or hardware assessment. In a fulfillment center environment, workstation reliability directly impacts operational throughput.

## Symptoms

- System running noticeably slower than normal
- Applications crash, freeze, or fail to launch
- Login takes excessively long or fails entirely
- Blue Screen of Death (BSOD) or kernel panic
- High fan noise or thermal throttling indicators
- Disk space warnings or write failures
- Peripheral devices (scanner, printer, monitor) not responding
- OS update failures or pending restart loops

## Likely Causes

1. **Resource exhaustion:** High CPU, memory, or disk utilization from runaway processes or insufficient hardware
2. **Disk space critically low:** System drive at or near capacity, preventing normal operations
3. **OS or driver issues:** Corrupted system files, failed updates, or incompatible drivers
4. **Malware or unwanted software:** Malicious or resource-heavy software consuming system resources
5. **Hardware failure:** Failing hard drive, bad RAM, overheating components, or peripheral malfunction
6. **Authentication or domain issues:** Account lockout, expired credentials, or domain controller unreachable

## Diagnostic Steps

1. **Check system resource utilization:**
   - Windows: Task Manager (Ctrl+Shift+Esc) > Performance tab
   - Linux: `top` or `htop` for CPU/memory; `df -h` for disk space
   - Identify any process consuming excessive CPU or memory

2. **Review recent changes:**
   - Were any updates installed recently?
   - Was new software installed or hardware changed?
   - Did the issue start at a specific time?

3. **Check disk space:**
   - Windows: File Explorer > This PC (check C: drive free space)
   - Linux: `df -h`
   - Critical threshold: less than 10% free space on system drive

4. **Review system logs:**
   - Windows: Event Viewer > Windows Logs > System and Application
   - Linux: `journalctl -xe` or `/var/log/syslog`
   - Look for: errors, warnings, crash dumps, hardware failures

5. **Check for pending updates or restart requirements:**
   - Windows: Settings > Update & Security
   - Linux: `apt list --upgradable` or `yum check-update`
   - Pending updates requiring restart can cause instability

6. **Run basic hardware diagnostics:**
   - Listen for unusual drive sounds (clicking, grinding)
   - Check system temperatures if accessible
   - Test with external peripherals disconnected to isolate hardware

## Resolution Steps

### Resource Exhaustion
1. Identify the high-consumption process in Task Manager or `top`
2. End non-essential processes consuming excessive resources
3. Check startup programs and disable unnecessary ones
4. If a specific application is consistently problematic, document and escalate for repair or replacement
5. Restart the device to clear accumulated resource leaks

### Disk Space Critically Low
1. Clear temporary files:
   - Windows: Disk Cleanup tool or `cleanmgr`
   - Linux: `sudo apt clean` or clear `/tmp`
2. Empty Recycle Bin / Trash
3. Identify large files: Windows: WinDirStat; Linux: `du -sh /* | sort -rh | head`
4. Remove or archive unnecessary files
5. If system-managed device, escalate for policy-compliant cleanup

### OS or Driver Issues
1. Run system file checker:
   - Windows: `sfc /scannow` (as administrator)
   - Linux: package verification tools
2. Check for and install pending updates
3. Roll back a recently installed driver if issue correlates with driver change
4. If BSOD, note the stop code and search for known fixes

### Hardware Failure
1. Run built-in hardware diagnostics (Dell ePSA, HP Diagnostics, etc.)
2. Check SMART status for hard drives: `wmic diskdrive get status` (Windows)
3. Test RAM with Windows Memory Diagnostic or `memtest86`
4. If hardware failure confirmed, document and escalate for replacement

### Authentication or Domain Issues
1. Verify network connectivity to domain controllers
2. Check if account is locked out (contact IT admin or use self-service)
3. Verify system clock is synchronized (Kerberos requires time accuracy)
4. Try logging in with a local account to isolate domain vs. local issue

## Escalation Notes

- **When to escalate:** If hardware failure is suspected, if the issue requires imaging or OS reinstallation, if domain/authentication infrastructure is involved, or if the issue affects a critical operational workstation
- **Information to provide:** Device hostname and asset tag, symptoms observed, diagnostic steps completed, Event Viewer errors (stop codes, error IDs), and operational impact (how many users affected, which processes are blocked)
- **Relevant teams:** Desktop support (hardware, imaging), systems administration (domain, authentication), application support (specific software issues)

## What I Learned

- Structured triage (check resources, check logs, check hardware) prevents wasted time on incorrect assumptions
- In fulfillment center environments, workstation downtime directly impacts operational metrics, making rapid diagnosis critical
- Many endpoint issues are resolved by restart, update installation, or disk cleanup, but documenting the root cause prevents recurrence
- Understanding the difference between symptoms and root causes is essential for effective escalation

## References

- CompTIA A+ troubleshooting methodology
- Linux and Private Cloud Administration: System monitoring and logging
