---
title: Firmware Update and Management Guide for HBA Controllers
created: 2024-10-30
modified: 2024-10-30
type: procedure
category:
  - storage
  - firmware
tags:
  - HBA
  - firmware-update
  - storcli
  - storage-management
---


# Firmware Update and Management for HBA/SAS Controllers Using StorCLI

## Overview

This guide provides a universal process for updating firmware and managing HBA/SAS controllers using the `storcli` tool. It includes steps for creating directories, transferring necessary files, and executing firmware updates and controller management commands across supported devices.

---

## Firmware Update Process

### 1. **Prepare Firmware Directory**
Create a directory on each server to store firmware files:

```bash
sudo mkdir -p /opt/firmware
```

### 2. **Transfer Firmware to Target Servers**
Use `scp` to copy the firmware file from a source server to each target server:

```bash
scp user@source_server:/path/to/firmware_file.bin /opt/firmware/
```

Replace `user@source_server` with your username and hostname/IP of the source server.

### 3. **Verify Firmware File Location**
Ensure the firmware file is in the correct directory on each server:

```bash
ls /opt/firmware/
```

You should see your firmware file listed.

### 4. **Install Firmware**
Run the firmware update using `storcli`:

```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 download file=/opt/firmware/firmware_file.bin
```

- `/c0` refers to **Controller 0**; adjust to `/c1`, `/c2`, etc., for other controllers if needed.

Repeat this process for each server as necessary.

---

## Managing `storcli` Across Servers

### 1. **Transfer `storcli64` to Target Servers**
If `storcli64` is missing on a server, copy it from another system where it’s available:

```bash
scp user@source_server:/opt/MegaRAID/storcli/storcli64 /opt/MegaRAID/storcli/
sudo chmod +x /opt/MegaRAID/storcli/storcli64
```

### 2. **Store `storcli64` in a Consistent Directory**
For uniformity, store `storcli64` in the following directory on all servers:

```bash
/opt/MegaRAID/storcli/
```

### 3. **Run `storcli64` Commands with `sudo`**
Use `sudo` for all `storcli64` commands, as controller management requires elevated privileges:

```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 show
```

---

## Common `storcli` Commands

1. **List All Controllers**
   ```bash
   sudo /opt/MegaRAID/storcli/storcli64 show
   ```
   Provides an overview of detected controllers with basic details like model and PCI address.

2. **Check Controller and Firmware Details**
   ```bash
   sudo /opt/MegaRAID/storcli/storcli64 /c0 show
   ```
   Displays detailed information on a specific controller, including firmware version, health status, and adapter model.

3. **Update Firmware**
   ```bash
   sudo /opt/MegaRAID/storcli/storcli64 /c0 download file=/opt/firmware/firmware_file.bin
   ```
   Downloads the specified firmware to the designated controller.

4. **Show Drive and RAID Information**
   ```bash
   sudo /opt/MegaRAID/storcli/storcli64 /c0 /eall /sall show all
   ```
   Lists all drives connected to the controller, including RAID configuration.

---

### Example Command Output for `/c0 show`

```
Controller = 0
Status = Success
Product Name = Broadcom / LSI SAS3008
Serial Number = XXXXXXX
FW Package Build = 50.0.0-1234
BIOS Version = 7.00.00.00
FW Version = 16.00.10.00
```
