---
title: "Smartctl Reference Guide for Storage Monitoring"
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: command-reference
category:
  - storage
  - monitoring
tags:
  - Smartctl
  - smartmontools
  - storage-monitoring
  - drive-health
  - zfs
---
## Description
The `smartctl` tool is part of the `smartmontools` suite and is essential for monitoring and analyzing SMART (Self-Monitoring, Analysis, and Reporting Technology) data from storage devices. This documentation focuses on commands that are useful in a ZFS environment to assess drive health, performance, and predict potential failures. Understanding these commands can help in proactive maintenance and ensuring data integrity in a storage pool.

## Commands / Snippets

> [!Warning] Use `sudo`
> In our environment, these commands require elevated access. Please use `sudo` to execute them.

### 1. Check Drive Information
- **Command**: `smartctl -i /dev/sdX`
- **Usage**: Displays identification information about the device, such as model, serial number, firmware version, which are useful for inventory and tracking.

### 2. Comprehensive Device Health Summary
- **Command**: `smartctl -a /dev/sdX`
- **Usage**: Provides a detailed SMART report including health status, all SMART attributes, error logs, and more. Essential for regular checks and audits.

### 3. Health Status Check
- **Command**: `smartctl -H /dev/sdX`
- **Usage**: Shows the SMART health status of the drive, which indicates if the drive is considered healthy or failing according to its SMART data.

### 4. Run SMART Tests
- **Command**: `smartctl -t short /dev/sdX`
- **Usage**: Initiates a short SMART self-test on the device. This test is quicker (usually around 1-2 minutes) and helps identify immediate issues.

### 5. View SMART Test Results
- **Command**: `smartctl -l selftest /dev/sdX`
- **Usage**: Lists the results of SMART self-tests performed on the device. Useful for reviewing past tests to detect changes over time.

### 6. Check Device Capabilities
- **Command**: `smartctl -c /dev/sdX`
- **Usage**: Displays what SMART capabilities and features the drive supports. This can help determine what kinds of monitoring and tests are possible with the device.

### 7. Monitor Device Temperature
- **Command**: `smartctl -A /dev/sdX | grep Temperature`
- **Usage**: Extracts temperature information from the detailed SMART attributes. Keeping an eye on drive temperature can prevent overheating and potential damage.

These commands form the core of SMART monitoring in a ZFS environment and provide vital information for maintaining the health and performance of storage devices. Regular monitoring using these commands can help in early detection of potential drive failures, contributing to overall system reliability.


