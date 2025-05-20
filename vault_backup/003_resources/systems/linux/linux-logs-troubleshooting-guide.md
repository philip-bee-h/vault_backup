---
title: linux-logs-troubleshooting-guide
created: 2024-12-03
type: command-reference
system: 
tags:
  - commands
  - linux
  - terminal
  - troubleshooting
related_runbooks:
---

## **General Troubleshooting Guide for Linux Logs**

### **1. `dmesg` (Kernel Messages)**

The `dmesg` command shows kernel messages, which are often the first place to look for hardware or kernel-level issues.  
**What to Look For:**

- **I/O Errors:** Indicate potential disk or filesystem issues.
- **Driver Failures:** Problems with hardware drivers.
- **OOM (Out of Memory):** Kernel killed processes due to memory pressure.
- **Hardware Warnings:** Failed devices, temperature warnings, etc.

#### **Useful Commands**

1. **Check for disk or filesystem errors:**

```bash
dmesg | grep -i "error"
```

**Explanation:** Searches for lines containing "error," which often point to failed I/O operations or hardware issues.

2. **Look for memory allocation issues:**

```bash
dmesg | grep -i "out of memory"
```

**Explanation:** Reveals if the kernel invoked the OOM killer to terminate processes due to memory exhaustion.

3. **Identify hardware failures:**

```bash
dmesg | grep -E "(fail|fault|warn|error)"
```

**Explanation:** Captures common keywords indicating failures or warnings across various subsystems.

4. **Filter for network-related issues:**

```bash
dmesg | grep -i "eth\|net\|tcp"
```

**Explanation:** Helps identify network interface or TCP/IP stack errors.

5. **Show boot errors:**

```bash
dmesg | grep -i "boot"
```

**Explanation:** Focuses on problems during the boot process, like missing modules or failed initializations.


---

### **2. System Event Log (SEL)**

SEL logs (often accessed via `ipmitool` or `journalctl`) capture hardware-related events from the system’s Baseboard Management Controller (BMC).

**What to Look For:**

- **Thermal Issues:** Overheating components.
- **Fan Failures:** System cooling issues.
- **Power Supply Errors:** Unstable or failed PSUs.
- **POST Failures:** Problems during power-on self-test.

#### **Useful Commands**

1. **View SEL entries:**

```bash
ipmitool sel list
```

**Explanation:** Displays all logged system events. Look for timestamps and descriptions of issues.

2. **Filter for errors or critical events:**

```bash
ipmitool sel list | grep -i "critical"
```

**Explanation:** Focuses on events marked critical, such as overheating or power failures.

3. **Show detailed error logs:**

```bash
ipmitool sel elist
```

**Explanation:** Provides more verbose output for deeper analysis.


---

### **3. System Journal (`journalctl`)**

The `journalctl` command provides a unified view of logs from `systemd`-based services.

**What to Look For:**

- Service crashes.
- Authentication failures.
- Resource exhaustion.

#### **Useful Commands**

1. **Show recent kernel messages:**

```bash
journalctl -k
```

**Explanation:** Equivalent to `dmesg` but includes timestamped logs and more detailed information.

2. **Filter errors and warnings:**

```bash
journalctl -p 3 -xb
```

**Explanation:** Shows errors (`-p 3`) and warnings from the current boot (`-xb`).

3. **Check for authentication failures:**

```bash
journalctl | grep -i "authentication failure"
```

**Explanation:** Highlights issues with user or service logins.

4. **Trace a service failure:**

```bash
journalctl -u <service_name>
```

**Explanation:** Focuses on logs for a specific service (e.g., `nginx`, `ssh`).


---

### **4. Application-Specific Logs**

Some services write to their own log files, commonly in `/var/log`. Always review these files for service-specific troubleshooting.

#### **Useful Commands**

1. **Find all log files for a service:**

```bash
sudo find /var/log -name "*<service_name>*"
```

**Explanation:** Locates logs specific to a given service.

2. **Monitor a log in real-time:**

```bash
tail -f /var/log/<logfile>
```

**Explanation:** Useful for observing activity as it happens, especially during testing or issue reproduction.

3. **Filter logs for keywords:**

```bash
grep -i "error" /var/log/<logfile>
```

**Explanation:** Quickly highlights errors in application logs.


---

### **5. Hardware and Resource Monitoring**

#### **Useful Commands**

1. **Check disk health (SMART):**

```bash
sudo smartctl -a /dev/sdX
```

**Explanation:** Provides a detailed health status of a disk. Look for reallocated sectors or read/write failures.

2. **Monitor hardware with `lshw`:**

```bash
sudo lshw -C memory -C network -C disk
```

**Explanation:** Focuses on memory, network interfaces, and storage devices to identify hardware issues.

3. **Find faulty drives in ZFS pools:**

```bash
zpool status
```

**Explanation:** Shows the health of ZFS pools. Look for `FAULTED` or `DEGRADED` status.


---

### **6. Additional Tips**

- **Understand Timestamps:** Correlate logs with the event’s timestamp to isolate when an issue occurred.
- **Use `less` for Navigation:** Combine `dmesg` or other log commands with `less` for easier browsing:

```bash
dmesg | less
```

**Navigation:** Use `/keyword` to search within the logs.
- **Combine Filters:** Combine `grep` and `awk` for more refined searches:

```bash
dmesg | grep -i "error" | awk '{print $1, $2, $3}'
```

**Explanation:** Extracts key timestamps and summaries.
