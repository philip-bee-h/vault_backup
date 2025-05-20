---
title: pgrep
created: 2025-01-31
type: command-reference
system: 
tags:
  - commands
  - unix
  - linux
  - macos
related_runbooks:
---
### `pgrep` Explanation:

`pgrep` searches for processes by name and returns their process IDs (PIDs). It's useful for finding running processes without manually filtering `ps` output.

### Recommended `pgrep` Commands for ZFS:

Here are a few useful `pgrep` commands related to ZFS:

1. **Check if ZFS-related processes are running:**
    
```bash
pgrep -l zfs
```
    
(Lists running ZFS-related processes)
    
2. **Find `zpool` processes:**
    
```bash
pgrep -fl zpool
```
    
(Searches for `zpool` commands running in the system)
    
3. **Monitor `zfs` send/receive processes:**
    
```bash
pgrep -fl "zfs send|zfs receive"
```
    
(Checks if a ZFS replication or backup operation is in progress)
    
4. **Find `scrub` or `resilver` processes:**
    
```bash
pgrep -fl "zpool scrub|zpool resilver"
```
    
(Ensures a scrub or resilver is actively running)
    

---

### `pgrep` Commands for **SGE (Sun Grid Engine)**

If you're working with **SGE (Son of Grid Engine/UGE)**, here are some useful `pgrep` commands:

1. **Check if the SGE `qmaster` daemon is running:**
    
```bash
pgrep -fl sge_qmaster
```
    
    (Ensures the Grid Engine master daemon is active)
    
2. **Check if the SGE execution daemon is running:**
    
```bash
pgrep -fl sge_execd
```
    
    (Verifies if the execution daemon is running on compute nodes)
    
3. **Find running `qsub` jobs:**
    
```bash
pgrep -fl qsub
```
    
    (Lists currently running job submission processes)
    
4. **Check if `sge_shepherd` (job execution process) is running:**
    
```bash
pgrep -fl sge_shepherd
```
    
    (Ensures job execution processes are actively running)
    
5. **Find users running SGE jobs:**
    
```bash
pgrep -fl -u $(whoami) sge_shepherd
```
    
    (Lists only your jobs running under `sge_shepherd`)
    

