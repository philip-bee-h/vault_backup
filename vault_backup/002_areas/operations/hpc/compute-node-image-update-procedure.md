---
title: Untitled
created: 2025-01-06
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
  - hpc
  - troubleshooting
related_docs:
---
### Resolution Steps

#### Verify the Current Compute Image:

```bash
wwsh provision list <hostName>
```

**Output:**

```bash
NODE                VNFS            BOOTSTRAP             FILES                
=================================================================================
argon-itf-cf46-04   compute_2024.2  compute_2024.2        group,gshadow,host...
```

#### Update the Compute Image for the Node:

```bash
wwsh provision set --bootstrap=compute_2024.3 --vnfs=compute_2024.3 argon-itf-cf46-04
```

#### Reboot the Node:

```bash
reboot
```

#### Run Puppet to Reconfigure:

**On the compute node:**

```bash
puppet agent --test
```

**On the head node:**

```bash
puppet agent --test
```

**Back to the compute node:**

```bash
puppet agent --test
```

#### Verify `sge_execd` is Running:

```bash
qhost -q -h argon-itf-cf46-04
```

**Expected output:**

```bash
HOSTNAME                ARCH         NCPU NSOC NCOR NTHR  LOAD  MEMTOT  MEMUSE  SWAPTO  SWAPUS
----------------------------------------------------------------------------------------------
argon-itf-cf46-04       lx-amd64      128    2   64  128  0.09    1.5T   16.3G    2.0G     0.0
```




---

### Issue Summary

A compute node (`argon-itf-cf46-04`) failed to join the queue despite Puppet showing no errors during its runs. Further investigation revealed that it also failed to mount `localscratch` due to an outdated compute image (`compute_2024.2`) being applied during provisioning. This led to cascading failures in dependent configurations, including `sge_execd`.

### Key Observations

#### Initial Symptoms:

- Puppet runs completed successfully with no reported errors.
- The node did not join the SGE queue (`sge_execd` was not running).

#### Further Investigation:

- **Output of `lsblk`:** Shows partitions but no mounted filesystems for `localscratch`.
- **Output of `df`:** No `/localscratch` mount point present.

```bash
[root@argon-itf-cf46-04 ~]# lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
nvme0n1     259:0    0  3.5T  0 disk 
|-nvme0n1p1 259:1    0    2G  0 part 
`-nvme0n1p2 259:2    0  3.5T  0 part 

[root@argon-itf-cf46-04 ~]# df -lh
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           756G  4.1G  752G   1% /
```

#### Expected Node Behavior:

A correctly configured compute node should have:

- `/nvme0n1p1` mounted as `[SWAP]`
- `/nvme0n1p2` mounted as `/localscratch`

```bash
[root@argon-itf-bx37-01 ~]# lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
nvme0n1     259:0    0  3.5T  0 disk 
|-nvme0n1p1 259:1    0    2G  0 part [SWAP]
`-nvme0n1p2 259:2    0  3.5T  0 part /localscratch
```

### Root Cause

The node was provisioned with an outdated compute image (`compute_2024.2`) that had a bug in the firstboot script, causing failures to mount `localscratch` and preventing the node from joining the queue.