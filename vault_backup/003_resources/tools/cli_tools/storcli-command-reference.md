---
title: storcli-command-reference
created: 2024-10-30
modified: 2024-10-30
type: command-reference
category:
  - storage
  - SAS-controller
tags:
  - StorCLI
  - RAID
  - SAS-controller
  - storage-management
  - reference
  - uiowa
---
Absolutely! Here’s the updated note with the link specified as the download page.

---

# StorCLI Quick Reference

## Description

`storcli` is a command-line tool for managing and configuring HBA and SAS controllers, providing access to controller and RAID management functions. This tool is essential for tasks such as checking controller status, managing firmware updates, and configuring RAID arrays.  
For downloads, refer to Intel’s [StorCLI Standalone Utility Download Page](https://www.intel.com/content/www/us/en/download/17809/storcli-standalone-utility.html).

---

## Path and Execution

**Binary Location**: `/opt/MegaRAID/storcli/storcli64`  
**Command Format**:  
```bash
sudo /opt/MegaRAID/storcli/storcli64 <options>
```

---

## Common Commands

### 1. **List All Controllers**
```bash
sudo /opt/MegaRAID/storcli/storcli64 show
```
   Displays all detected controllers, including basic information such as model and PCI address.

### 2. **Check Controller Status**
```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 show
```
   Shows detailed information about **Controller 0** (adjust `/c0` as needed), including model, firmware, and health status.

### 3. **Update Firmware**
```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 download file=/opt/firmware/firmware_file.bin
```
   Updates firmware for **Controller 0** using the specified firmware file.

### 4. **List Drives and RAID Information**
```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 /eall /sall show all
```
   Displays details for all drives and RAID configurations connected to **Controller 0**.

### 5. **Create a Virtual Drive**
```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 add vd type=raid0 drives=0:1
```
   Creates a RAID 0 virtual drive using drives specified in the format `enclosure:slot` (e.g., `0:1`).
