---
title: locating-disk-info
created: 2025-04-21
type: command-reference
system: 
tags:
  - commands
  - hardware
  - storage
related_runbooks:
---

## Reference: Finding Disk Info When `smartctl` Doesn't Show a Serial

### 1. **Use symlinks in `/dev/disk/by-id/` to find logical disk name**

This directory contains symlinks that map persistent device identifiers to device nodes (e.g., `/dev/sdaj`).

```bash
ls -l /dev/disk/by-id/ | grep <disk_id_or_hint>
```

**Example:**

```bash
ls -l /dev/disk/by-id/ | grep wwn-0x5000cca2914c3514
# Output:
# wwn-0x5000cca2914c3514 -> ../../sdaj
```

This tells you the device is `/dev/sdaj`.

---

### 2. **Use `lshw` to get detailed disk info**

Once you know the device name (e.g., `sdaj`), use `lshw` to find more info like vendor, product, and serial.

#### Basic grep:

```bash
sudo lshw -class disk | grep sdaj
```

#### Extended context (8 lines before and after):

```bash
sudo lshw -class disk | grep -8 sdaj
```

**Example output:**

```
  *-disk:29
       description: SCSI Disk
       product: HUH721212AL5200
       vendor: HGST
       ...
       logical name: /dev/sdaj
       serial: 5PHAX7UE
```

---

### Notes:

- This method helps identify drive serials when tools like `smartctl` don't return them properly.
    
