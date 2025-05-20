---
title: Untitled
created: 2024-12-02
type: command-reference
system: 
tags:
  - commands
  - uiowa
related_runbooks:
---
troubleshooting.

  

# 1. General System Health Checks

  

Commands

  

• top or htop: Monitor CPU, memory, and I/O usage.

• Look for: High load averages, memory saturation, or high I/O wait.

• uptime: Quick check of system load and uptime.

• Look for: Consistently high load averages.

• vmstat 1: Detailed resource usage.

• Look for: High numbers in wa (I/O wait) or si (swap-in activity).

• iostat -x 1 (from sysstat package): Disk I/O statistics.

• Look for: High %util (utilization) or await (average wait time).

  

2. Filesystem and Disk Space

  

Commands

  

• df -h: Check free disk space on mounted filesystems.

• Look for: Filesystems nearing 100% usage.

• du -sh /path/to/directory: Check disk usage for a specific directory.

• Look for: Directories with unexpected high usage.

• find /path -type f -size +1G: Find large files.

• Look for: Large, unnecessary files eating up space.

• lsblk: View block devices and their mount points.

• Look for: Mismatched or unmounted partitions.

• blkid: Identify filesystem types and UUIDs.

• Look for: Missing or mismatched filesystem IDs.

  

3. Disk and Storage Health

  

Commands

  

• smartctl -a /dev/sdX (from smartmontools): Check SMART status of drives.

• Look for: Failing attributes like reallocated sectors, pending sectors, or errors.

• mdadm --detail /dev/mdX (if using RAID): Check RAID array status.

• Look for: degraded or faulty states.

• zpool status (if using ZFS): Check the status of ZFS pools.

• Look for: DEGRADED, OFFLINE, or UNAVAIL devices.

• lsof /mnt/mountpoint: Check for open files on a specific mount.

• Look for: Processes keeping a filesystem busy.

• pvs, vgs, lvs: Inspect LVM structures.

• Look for: Misaligned or full physical/logical volumes.

  

4. Troubleshooting Performance Bottlenecks

  

Commands

  

• iotop: Show real-time disk I/O per process.

• Look for: Processes with high I/O usage.

• dstat --top-io: Highlight top I/O processes.

• Look for: Persistent high disk write/read by specific processes.

• sar -d 1: Historical and real-time I/O metrics.

• Look for: Disks with consistently high usage.

• fstrim -v /mountpoint: Check or run TRIM on SSDs.

• Look for: Inefficiencies due to lack of TRIM usage.

  

5. Logs and Error Checking

  

Commands

  

• journalctl -p 3 -xb: View system-critical errors.

• Look for: Errors mentioning disk, fs, or mount.

• dmesg | grep -i error: Check kernel errors.

• Look for: Disk errors, file system errors, or hardware issues.

• grep -i "error|fail" /var/log/*: General log scan for issues.

• Look for: Repeated or storage-related messages.

  

6. Mount Points and Filesystems

  

Commands

  

• mount | grep /mnt/mountpoint: Confirm filesystem is mounted.

• Look for: Missing or incorrect mount points.

• cat /etc/fstab: Check for filesystem auto-mount configuration.

• Look for: Incorrect entries or missing filesystems.

• fsck /dev/sdX: Check and repair filesystems (use with caution on mounted filesystems).

• Look for: Filesystem inconsistencies or errors.

  

7. Network and NFS Troubleshooting (if applicable)

  

Commands

  

• showmount -e nfs-server: List exported NFS shares.

• Look for: Missing or unshared exports.

• mount | grep nfs: Verify mounted NFS shares.

• Look for: Unmounted or stale NFS mounts.

• nfsstat -s: Show server-side NFS statistics.

• Look for: High retransmissions or timeout counts.

• rpcinfo -p nfs-server: Check NFS server RPC services.

• Look for: Missing or unavailable RPC services.

  

8. Debugging with grep and awk

  

Common Patterns

  

• grep -i error /var/log/syslog: Look for errors in system logs.

• grep "disk read failed" /var/log/*: Search all logs for specific disk errors.

• awk '$1 == "cpu"' /proc/stat: Parse CPU stats.

• grep -E "sda|sdb" /proc/mdstat: Verify RAID components are present.

  

Pipe Examples

  

• df -h | grep -E '9[0-9]%': Find filesystems over 90% usage.

• iostat -x | awk '$1 == "sda" {print $1, $4, $5}': Extract useful disk metrics.

  

9. Useful Recommendations

  

• Automate Monitoring: Use tools like Prometheus, Grafana, or Nagios to monitor disk health, I/O performance, and usage trends.

• Document Known Issues: Track specific error patterns, their root causes, and solutions in your knowledge base.

• Keep a Runbook: For common problems (e.g., disk replacements or filesystem repair), write step-by-step instructions.

• Regular Backups: Automate backups to avoid data loss in case of critical storage failures.