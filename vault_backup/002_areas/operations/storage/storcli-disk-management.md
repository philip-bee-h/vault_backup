---
title: storcli-disk-management
created: 2024-10-30
modified: 2024-10-30
type: command-reference
category:
  - storage
  - SAS-controller
tags:
  - StorCLI
  - drive-management
  - SAS-controller
  - storage
  - reference
---

## **StorCLI Reference Guide for Viewing and Locating Drives**

This guide provides instructions for using the `storcli` tool to manage SAS controllers and drives on our servers. It covers steps for copying the tool between servers, checking controller information, viewing drive details (including WWN), and locating drives with blinking LEDs.

---

### 1. **Transferring `storcli64` Between Servers Using `scp`**

When `storcli` is not installed on a server, you can copy it from a server that already has it. Use the `scp` command **from the destination server** to pull the file from the source server.

#### Example:

```bash
sudo scp user@source_server:/opt/MegaRAID/storcli/storcli64 /opt/MegaRAID/storcli/
```

- Replace `user@source_server` with your username and the source server's hostname or IP.
- Ensure the file is executable:

  ```bash
  sudo chmod +x /opt/MegaRAID/storcli/storcli64
  ```

---

### 2. **File Location in the Filesystem**

For consistency, store the `storcli64` binary in the following directory on all servers:

```bash
/opt/MegaRAID/storcli/
```

This ensures that the tool is easily accessible for SAS controller management tasks.

---

### 3. **Running `storcli64` with `sudo`**

Since managing SAS controllers requires elevated privileges, run `storcli64` with `sudo`.

#### Example:

```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 show
```

---

### 4. **Common StorCLI Commands**

#### 4.1 **List All Controllers**

To get an overview of all controllers in the system:

```bash
sudo /opt/MegaRAID/storcli/storcli64 show
```

---

### 5. **Viewing Drive Information and Status**

#### 5.1 **List All Drives**

To list all drives connected to a controller:

```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 /eall /sall show
```

- `/c0` refers to **Controller 0** (adjust for your controller ID).

This command provides a summary of all drives, including:
- Enclosure ID
- Slot number
- Drive model and type
- Status and health information

#### 5.2 **View Detailed Information about a Specific Drive**

To see detailed information about a particular drive, including the **World Wide Name (WWN)**, use:

```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 /eX /sY show all
```

- `/c0`: Controller ID.
- `/eX`: Enclosure ID.
- `/sY`: Slot number.

This will display detailed information such as:
- Drive capacity
- Drive status (Healthy/Faulted)
- Serial number
- **World Wide Name (WWN)**, useful for unique drive identification
- Temperature
- Power-on hours

The **WWN** is especially helpful when mapping drives to their physical or virtual location in the server.

---

### 6. **Locating Drives by Blinking LEDs**

#### 6.1 **Locate a Drive with LED**

To physically identify a drive by blinking its LED:

```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 /eX /sY start locate
```

- This command will blink the LED on the specified drive (Enclosure `X`, Slot `Y`).

#### 6.2 **Stop Blinking LED**

Once you've identified the drive, stop the LED blinking with:

```bash
sudo /opt/MegaRAID/storcli/storcli64 /c0 /eX /sY stop locate
```

---

### 7. **Example Workflow for Viewing and Locating Drives**

1. **List All Drives**:
   ```bash
   sudo /opt/MegaRAID/storcli/storcli64 /c0 /eall /sall show
   ```

2. **View Detailed Drive Info (Enclosure 0, Slot 2, including WWN)**:
   ```bash
   sudo /opt/MegaRAID/storcli/storcli64 /c0 /e0 /s2 show all
   ```

3. **Blink LED for Drive (Enclosure 0, Slot 2)**:
   ```bash
   sudo /opt/MegaRAID/storcli/storcli64 /c0 /e0 /s2 start locate
   ```

4. **Stop LED Blink**:
   ```bash
   sudo /opt/MegaRAID/storcli/storcli64 /c0 /e0 /s2 stop locate
   ```

---

### Example Command Output
Here is an example output for the `/c0 /e0 /s2 show all` command, including WWN:

```
Enclosure Device ID = 0
Slot Number = 2
Device ID = 7
WWN = 5000C500A1B2C345
State = Online
...
```

