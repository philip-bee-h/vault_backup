---
title: zpool-smartctl-a-script
created: 2024-12-09
type: command-reference
system: 
tags:
  - commands
  - uiowa
related_runbooks:
---
**Modified:**



## smartctl -t /dev/disk script

```sh
#!/bin/bash

# Define the ZFS pool name
POOL_NAME="dpool01"

# Extract FAULTED and DEGRADED disk identifiers from zpool status
DISKS=$(zpool status "$POOL_NAME" | awk '/FAULTED|DEGRADED/ {print $1}')

# Check if any faulted or degraded disks are found
if [ -z "$DISKS" ]; then
    echo "No FAULTED or DEGRADED disks found in pool $POOL_NAME."
    exit 0
fi

# Function to determine disk path (by-id or by-path)
get_disk_path() {
    local DISK=$1
    if [ -e "/dev/disk/by-id/$DISK" ]; then
        echo "/dev/disk/by-id/$DISK"
    elif [ -e "/dev/disk/by-path/$DISK" ]; then
        echo "/dev/disk/by-path/$DISK"
    else
        echo ""
    fi
}

# Run a short SMART test on each faulted or degraded disk
for DISK in $DISKS; do
    DISK_PATH=$(get_disk_path "$DISK")
    
    if [ -z "$DISK_PATH" ]; then
        echo "Disk $DISK not found by-id or by-path. Skipping."
        continue
    fi

    echo "Running SMART short test on disk: $DISK ($DISK_PATH)"
    if sudo smartctl -t short "$DISK_PATH"; then
        echo "SMART short test initiated successfully for $DISK ($DISK_PATH)."
    else
        echo "Failed to initiate SMART short test for $DISK ($DISK_PATH)."
    fi
done

echo "SMART short test initiated for all FAULTED and DEGRADED disks."
```



## **smartctl -a /dev/disk/ script**


```sh
#!/bin/bash

# Define the ZFS pool name
POOL_NAME="dpool01"

# Define the combined log file
LOG_FILE="smartctl_combined.log"

# Clear the log file at the start
true > "$LOG_FILE"

# Extract FAULTED and DEGRADED disks from zpool status
DISKS=$(zpool status "$POOL_NAME" | awk '/FAULTED|DEGRADED/ {print $1}')

# Check if any faulted or degraded disks are found
if [ -z "$DISKS" ]; then
    echo "No FAULTED or DEGRADED disks found in pool $POOL_NAME."
    exit 0
fi

# Function to determine disk path (by-id or by-path)
get_disk_path() {
    local DISK=$1
    if [ -e "/dev/disk/by-id/$DISK" ]; then
        echo "/dev/disk/by-id/$DISK"
    elif [ -e "/dev/disk/by-path/$DISK" ]; then
        echo "/dev/disk/by-path/$DISK"
    else
        echo ""
    fi
}

# Loop through each disk and log the smartctl -a output
for DISK in $DISKS; do
    DISK_PATH=$(get_disk_path "$DISK")
    
    if [ -z "$DISK_PATH" ]; then
        echo "Disk $DISK not found by-id or by-path. Skipping." >> "$LOG_FILE"
        continue
    fi

    {
        echo "Logging SMART details for disk: $DISK ($DISK_PATH)"
        echo "=================================================="
        if sudo smartctl -a "$DISK_PATH"; then
            echo "SMART data for $DISK logged successfully."
        else
            echo "Failed to log SMART data for $DISK."
        fi
        echo ""  # Add a blank line for readability
    } >> "$LOG_FILE" 2>&1
done

echo "SMART data logging completed. Combined log is saved in $LOG_FILE."
```