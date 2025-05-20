---
title: disk-model-serial-slot-led
created: 2025-04-04
type: command-reference
system: 
tags:
  - commands
  - zfs
related_runbooks:
---
# Locating Disks by Model, Serial, Enclosure Slot, and LED

This outlines how to identify disks using `udevadm`, determine their slot location, and blink LEDs for physical tracing.

---

## 1. Get Disk Info (Model, Serial, Path)

Use `udevadm` with either the WWN or path to the disk:

```bash
udevadm info --query=all --name=/dev/disk/by-id/wwn-0x5000cca2917a4f38
```

Or by PCI path:

```bash
udevadm info --query=all --name=/dev/disk/by-path/pci-0000:af:00.0-sas-0x5000cca2917a4f39-lun-0
```

Look for the following fields in output:

- `ID_MODEL=` – drive model
    
- `ID_SERIAL_SHORT=` – serial number (usually WWN)
    
- `ID_WWN=` – unique identifier
    
- `ID_PATH=` – PCI SAS path
    
- `DEVNAME=` – the actual device (e.g., `/dev/sdb`)
    

---

## 2. Find Enclosure Slot Location

ZFS exposes physical enclosure slots via sysfs if SES is supported.

Example path for a slot:

```
/sys/class/enclosure/16:0:37:0/Slot09/
```

Use `zdb -C` to see which disk is assigned to which slot via `vdev_enc_sysfs_path`.

You can also list all enclosure slots:

```bash
ls /sys/class/enclosure/16:0:37:0/
```

---

## 3. Blink LED to Physically Identify a Drive

### Turn on locate LED:

```bash
echo 1 | sudo tee /sys/class/enclosure/16:0:37:0/Slot09/locate
```

### Turn off locate LED:

```bash
echo 0 | sudo tee /sys/class/enclosure/16:0:37:0/Slot09/locate
```

### Optional: Blink fault LED (if supported)

```bash
echo 1 | sudo tee /sys/class/enclosure/16:0:37:0/Slot09/fault
```

Turn it off:

```bash
echo 0 | sudo tee /sys/class/enclosure/16:0:37:0/Slot09/fault
```

---

## Summary

- Use `udevadm` to get serial/model/WWN
    
- Use `zdb -C` and `/sys/class/enclosure/` to match disks to slots
    
- Use `locate` or `fault` to blink LEDs for physical disk identification
    

This method is ideal for drive swaps, verifying physical layout, or audits.

