---
title: "zfs-zpool.d-scripts-ref"
created: "2025-02-21"
type: command-reference
system: 
tags:
  - commands
  - uiowa
related_runbooks:
---
# **ZFS `zpool.d` Scripts Documentation**

> These `zpool.d` scripts provide additional diagnostics and metadata retrieval for ZFS pools. They should be available on all storage and LSS servers running ZFS, located in `/usr/libexec/zfs/zpool.d/`. Use them with `zpool status -c` or `zpool iostat -c` to extract device details, health status, and performance metrics.

## Accessing and Running `zpool.d` Scripts

### Listing Available Scripts

To view all available ZFS `zpool.d` scripts, run:

```bash
ls -l /etc/zfs/zpool.d
```

or

```bash
ls -l /usr/libexec/zfs/zpool.d
```

### Running a Script

The `zpool.d` scripts are **not standalone commands** but are used with `zpool status -c` or `zpool iostat -c`.

#### Example Usage with `dpool01`:

To retrieve the serial number of devices in `dpool01`:

```bash
zpool status -c serial dpool01
```

To retrieve multiple properties (e.g., serial, model, and health):

```bash
zpool status -c serial,model,health dpool01
```

For I/O statistics using a script:

```bash
zpool iostat -c iostat dpool01
```

## `zpool.d` Script Descriptions

### SMART Statistics

- **smart** - Show SMART temperature and error stats (specific to drive type).
- **smartx** - Show SMART extended drive stats (specific to drive type).
- **temp** - Show SMART drive temperature in Celsius (all drives).
- **health** - Show reported SMART status (all drives).
- **r_proc** - Show SMART read GB processed over drive lifetime (SAS).
- **w_proc** - Show SMART write GB processed over drive lifetime (SAS).
- **r_ucor** - Show SMART read uncorrectable errors (SAS).
- **w_ucor** - Show SMART write uncorrectable errors (SAS).
- **nonmed** - Show SMART non-medium errors (SAS).
- **defect** - Show SMART grown defect list (SAS).
- **hours_on** - Show number of hours drive powered on (all drives).
- **realloc** - Show SMART reallocated sectors count (ATA).
- **rep_ucor** - Show SMART reported uncorrectable count (ATA).
- **cmd_to** - Show SMART command timeout count (ATA).
- **pend_sec** - Show SMART current pending sector count (ATA).
- **off_ucor** - Show SMART offline uncorrectable errors (ATA).
- **ata_err** - Show SMART ATA errors (ATA).
- **pwr_cyc** - Show SMART power cycle count (ATA).
- **serial** - Show disk serial number.
- **nvme_err** - Show SMART NVMe errors (NVMe).

### SMART Self-Test Information

- **smart_test** - Show SMART self-test results summary.
- **test_type** - Show SMART self-test type (short, long, etc.).
- **test_status** - Show SMART self-test status.
- **test_progress** - Show SMART self-test percentage done.
- **test_ended** - Show when the last SMART self-test ended (if supported).

### SCSI Enclosure Services (SES) Information

- **enc** - Show disk enclosure w:x:y:z value.
- **slot** - Show disk slot number as reported by the enclosure.
- **encdev** - Show `/dev/sg*` device associated with the enclosure disk slot.
- **fault_led** - Show value of the disk enclosure slot fault LED.
- **locate_led** - Show value of the disk enclosure slot locate LED.
- **ses** - Show disk's enc, enc device, slot, and fault/locate LED values.

This documentation provides a quick reference for using `zpool.d` scripts with the ZFS pool `dpool01`. Let us know if you need further details!


q