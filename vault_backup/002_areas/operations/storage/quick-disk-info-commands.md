---
title: quick-disk-info-commands
created: 2024-12-12
type: command-reference
system: 
tags:
  - commands
  - uiowa
  - storage
  - storage-management
  - terminal
related_runbooks:
---
### Quick Disk Info Reference

#### Description:
- Common commands for locating disk information

#### List Disk Paths by Connection

```bash
ls -l /dev/disk/by-path/
```

#### View Disk Details in a Table

```bash
lsblk -o NAME,KNAME,FSTYPE,LABEL,MODEL,SERIAL,SIZE,HCTL
```

**Example Output:**

```sh
NAME    KNAME FSTYPE     LABEL  MODEL       SERIAL         SIZE HCTL
sdk     sdk             MB8000JFECQ 5000c50093fddc5f 7.3T  7:0:11:0
├─sdk1  sdk1 zfs_member dpool01                            7.3T
└─sdk9  sdk9                                                  8M
```

#### Get Detailed Disk Info

```bash
udevadm info --query=all --name=/dev/sdk
```

**Example Output:**

```sh
[hawrth@itf-iihg-stor01 ~]$ udevadm info --query=all --name=/dev/sdk
P: /devices/pci0000:00/0000:00:03.2/0000:03:00.0/host7/port-7:12/end_device-7:12/target7:0:11/7:0:11:0/block/sdk
N: sdk
S: disk/by-id/scsi-35000c50093fddc5f
S: disk/by-id/wwn-0x5000c50093fddc5f
S: disk/by-path/pci-0000:03:00.0-sas-0x5000c50093fddc5d-lun-0
E: DEVLINKS=/dev/disk/by-id/scsi-35000c50093fddc5f /dev/disk/by-id/wwn-0x5000c50093fddc5f /dev/disk/by-path/pci-0000:03:00.0-sas-0x5000c50093fddc5d-lun-0
E: DEVNAME=/dev/sdk
E: DEVPATH=/devices/pci0000:00/0000:00:03.2/0000:03:00.0/host7/port-7:12/end_device-7:12/target7:0:11/7:0:11:0/block/sdk
E: DEVTYPE=disk
E: ID_BUS=scsi
E: ID_MODEL=MB8000JFECQ
E: ID_MODEL_ENC=MB8000JFECQ\x20\x20\x20\x20\x20
E: ID_PART_TABLE_TYPE=gpt
E: ID_PATH=pci-0000:03:00.0-sas-0x5000c50093fddc5d-lun-0
E: ID_PATH_TAG=pci-0000_03_00_0-sas-0x5000c50093fddc5d-lun-0
E: ID_REVISION=HPD7
E: ID_SCSI=1
E: ID_SCSI_SERIAL=ZA17WDAG
E: ID_SERIAL=35000c50093fddc5f
E: ID_SERIAL_SHORT=5000c50093fddc5f
E: ID_TYPE=disk
E: ID_VENDOR=HP
E: ID_VENDOR_ENC=HP\x20\x20\x20\x20\x20\x20
E: ID_WWN=0x5000c50093fddc5f
E: ID_WWN_WITH_EXTENSION=0x5000c50093fddc5f
E: MAJOR=8
E: MINOR=160
E: SUBSYSTEM=block
E: TAGS=:systemd:
E: USEC_INITIALIZED=98929
```


\