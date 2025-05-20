---
title: zdb-zfs-command
created: 2025-04-04
type: command-reference
system: 
tags:
  - commands
  - zfs
related_runbooks:
---

# `zdb` Reference Guide

The `zdb` command is a low-level **ZFS debugger** tool used to inspect internal structures, metadata, vdevs, labels, and cached configuration information.

> **Caution:** `zdb` is a read-only tool but not intended for casual use — best reserved for debugging, inspection, or recovery.

---

## Common Usage Patterns

### 1. View Cached Pool Configuration

```bash
zdb -C
```

- Shows the pool configuration from the **zpool cache file**.
    
- Useful for seeing vdev layout and device info **without importing the pool**.
    

---

### 2. Dump ZFS Labels from a Device

```bash
zdb -l /dev/disk/by-id/<device>
```

- Reads and prints **ZFS label metadata** directly from a device.
    
- Helpful for identifying which pool a disk belongs to, especially in recovery scenarios.
    

---

### 3. Inspect a Specific Pool

```bash
zdb <poolname>
```

- Shows a **summary of the pool**, including:
    
    - Dataset hierarchy
        
    - Vdev configuration
        
    - Object set info
        

---

## Additional `zpool` Commands for Comparison

|Command|Description|
|---|---|
|`zpool status`|Health and status of pools and devices|
|`zpool iostat -v`|Per-vdev I/O statistics|
|`zpool list -v`|Space usage per vdev|
|`zpool get all <poolname>`|Full list of pool properties|

---

## ZFS Cache File Inspection

### View contents of the pool cache:

```bash
cat /etc/zfs/zpool.cache
```

### Or with strings for readability:

```bash
strings /etc/zfs/zpool.cache | less
```

> On systems with systemd and ZFS boot integration:

```bash
/boot/zfs/zpool.cache
```

---

## Notes

- `zdb` does not modify pools or devices.
    
- It's ideal for forensic analysis or offline inspection.
    
- Output can be verbose — pipe it to `less` or redirect to a file for easier review.
    
