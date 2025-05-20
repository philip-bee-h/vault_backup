---
title: dead-node-investigating-process
created: 2024-10-28
modified: 2024-10-28
type: process
system:
  - hpc
tags:
  - troubleshooting
  - hpc
  - argon
  - process
  - commands
  - sge
---


## Suggested Location

This process fits well under your operations area for HPC:

```
002_areas/
└── operations/
    └── hpc/
        └── argon-node-troubleshooting-process.md
```

---

## Process Order for Investigating Down Argon Compute Nodes

### 1. Log in to Argon Head
```bash
ssh argon-head.hpc.uiowa.edu
```

### 2. Network Communication
- **Ping/SSH**: Attempt to ping the node and SSH into it.
  - If successful, skip to steps 5 and 6.
  - If not reachable, proceed to step 3.

### 3. Power and Hardware Diagnostics
- **Check Power Status and SEL Errors**:
  - Use `ipmitool` to verify the power status and review the System Event Log (SEL) for errors.
  - **Commands**:

  - Checking power status:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi chassis status
```

  - If you encounter a LAN error, try:
```bash
sudo ipmitool -I lanplus -f /root/ipmipass -U ARGON -H <hostname>.ipmi chassis status
```

  - Reviewing error logs:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi sel elist | tail
```

### 4. Power Cycle and Re-check
- **IPMI and SEL Verification**:
  - If IPMI is responsive and no critical errors are in the SEL:  
- **Power Cycle the Node**:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi chassis power cycle
```
- **Repeat Step 2**: After the node powers up, repeat the network checks.
  - If IPMI is unresponsive, hardware errors are present in SEL, or if power cycling does not resolve the issue:
    - **Decommission Process**: Begin decommissioning procedures for the problematic node.

### 5. Confirm SGE Deployment
- Check if the SGE service is running:
```bash
ps -ef | grep sge
```

### 6. Confirm Queue is Up and Ready
- Verify that the queue is active:
```bash
qhost -q -h <hostname>
```

---

### Updated Troubleshooting Table

| Step | Action Description               | Command Used                                  |
|------|----------------------------------|-----------------------------------------------|
| 1    | Ping/SSH to the system           | `ping <hostname>` / `ssh <hostname>`          |
| 2    | Check power status and SEL errors | `ipmitool chassis status` / `ipmitool sel elist` |
| 3    | Power cycle and re-check          | `ipmitool chassis power cycle`                |
| 4    | Confirm SGE deployment            | `ps -ef | grep sge`                           |
| 5    | Confirm queue status              | `qhost -q -h <hostname>`                      |

---

### Additional Notes

For additional queue diagnostics, use:
```shell
qstat -explain aAcE -q INFORMATICS
```

This updated structure clarifies each troubleshooting step with accurate commands, and the table aligns with the process flow. Let me know if further adjustments are needed!

