---
title: my-zfs-disk-swap-runbook
created: 2025-03-13
type: command-reference
system: 
tags:
  - commands
  - uiowa
  - runbook
  - zfs
related_runbooks:
---
## ZFS Disk Swap Commands

```bash
ps -ef | grep zfs | wc -l
```

```bash
sudo /sbin/ledctl locate=/dev/disk/by-id/wwn-0x5000cca2848e494c
```

```bash
sudo /sbin/ledctl locate_off=/dev/disk/by-id/wwn-0x5000cca2848e494c
```

```bash
ls -alh /dev/disk/by-id | grep "$(date '+%b %d')"
```

```bash
ls -alh /dev/disk/by-id | grep "Mar  7"
```

```bash
sudo zpool replace dpool01 wwn-0x5000cca2848e494c /dev/disk/by-id/wwn-0x5000cca2bb426b60
```
