---
title: zfs-list-command-reference
created: 2025-05-19
type: command-reference
system: 
tags:
  - commands
  - zfs
  - storage
related_runbooks:
---

## ZFS: `zpool list` Reference

### Command:

```bash
zpool list
```

### Description:

Displays summary information about all ZFS storage pools on the system.

---

### Output Columns:

|Column|Description|
|---|---|
|**NAME**|Name of the ZFS pool.|
|**SIZE**|Total physical size of the pool (including parity, spares, etc.).|
|**ALLOC**|Amount of space currently allocated/used from the pool.|
|**FREE**|Amount of free/unallocated space remaining in the pool.|
|**CKPOINT**|Amount of space saved as part of a checkpoint (if any). Usually `-` if unused.|
|**EXPANDSZ**|Potential additional size available by expanding devices. Usually `-`.|
|**FRAG**|Fragmentation level of the pool (data fragmentation, not file fragmentation).|
|**CAP**|Pool capacity used as a percentage (`ALLOC` / `SIZE`). High values can affect performance.|
|**DEDUP**|Deduplication ratio (e.g., `1.00x` means no dedup savings).|
|**HEALTH**|Current health status of the pool (`ONLINE`, `DEGRADED`, `FAULTED`, etc.).|
|**ALTROOT**|Alternate root directory for the pool mount (usually `-` unless set).|

---

### Notes:

- Pools should ideally remain below **80-85%** capacity to avoid performance degradation.
    
- Check **HEALTH** regularly for any signs of hardware or configuration issues.
    
- Use `zpool status` for more detailed diagnostics.
    
