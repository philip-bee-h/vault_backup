---
title: "hpc-nodes-troubleshooting-reference-guide"
created: 2024-10-28
modified: 2024-10-28
type: troubleshooting
system: hpc
tags:
  - hpc
  - troubleshooting
  - diagnostics
  - ipmi
---
### HPC Nodes Troubleshooting Reference Guide

This guide is designed to assist with the management and troubleshooting of High-Performance Computing (HPC) nodes. It includes essential commands and structured steps for diagnosing, resolving, and verifying issues on HPC nodes.

---

### 1. Quick Access Commands

**Essential commands for immediate diagnostics and status checks.**

#### **1.1 Basic Connectivity and Status Checks**

- **Check Dead Nodes in Cluster**:
    
```bash
dead-nodes
```
    
- **SSH into Node**:
    
```bash
ssh argon-itf-bx56-35
```
    
- **Ping Test**:
    
```bash
ping argon-itf-bx49-37.hpc
```
    

---

### 2. System Grid Engine (SGE) Commands

**Commands related to job management and queue diagnostics within SGE.**

#### **2.1 Node and Queue Management**

- **List Nodes in Specific Queues**:
    
```shell
qselect -q <queue name>
```
    
- **List Node Names in Queue**:
    
```bash
qselect-node-list -q <queue name>
```
    
- **Node Details in Queue**:
    
```bash
qhost -h $(qselect-node-list -q <queue name>)
```
    

#### **2.2 Error Analysis Using SGE**

- **Finding Node Errors**:
    
```bash
qstat -f | awk '$6~/[cdsuE]/'
```
    
- **Queue and Job Status**:
    - **Queue**: `qhost -h <hostname> -q`
    - **Jobs**: `qhost -h <hostname> -j`

#### **2.3 Detailed Node Status with Explanation**

- **Check extended node status and alarms**:
    
```bash
qstat -explain a
```
    
    **Example Output:**
    
```
all.q@argon-itf-ca37-33.hpc    BIPC  0/0/40         0.18     lx-amd64      a
		alarm hl:np_load_avg=0.004500 load-threshold=1.5
		alarm hl:scratch_used_pct= 9 load-threshold=90
		alarm hl:swap_used=0.000000M load-threshold=1024M
		alarm hl:tmp_used_pct= 6 load-threshold=90
		error: no complex attribute for threshold nfsscratch_used_pct
```
    
**Node Status Legend:**

- `'au'` - Host is in alarm and unreachable.
- `'u'` - Host is unreachable (SGE or machine may be down).
- `'a'` - Host is in alarm (normal if fully utilized).
- `'aS'` - Host is in alarm and Suspended due to high resource usage.
- `'d'` - Host is disabled.
- `'E'` - Host is in ERROR state (use `qmod -c` to clear).

#### **2.4 Job Accounting with qacct**

- **Summary of qacct Commands:**
    
|Command|Description|
|---|---|
|`qacct`|Show overall job statistics|
|`qacct -j <job_id>`|Show details for a specific job|
|`qacct -o <username>`|Show jobs for a specific user|
|`qacct -q <queue_name>`|Show jobs for a queue|
|`qacct -h <host>`|Show jobs that ran on a specific host|
|`qacct -d <days>`|Show jobs from the last `<days>` days|
|`qacct -o <user> -t`|Summarize total CPU and memory usage|
    

---

### 3. IPMItool Hardware Diagnostics

**Commands for hardware-level diagnostics using IPMItool.**

#### **3.1 Basic Hardware Checks**

- **Check Chassis Status**:
    
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H argon-itf-bx49-37.ipmi chassis status
```
    
- **List Recent System Event Logs**:
    
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H argon-itf-bx48-36.ipmi sel elist | tail
```
    
- **Identify Chassis for Troubleshooting**:
    
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H argon-itf-bx48-36.ipmi chassis identify
```
    

---

### 4. Detailed Troubleshooting Procedures

**Guidelines for more comprehensive troubleshooting.**

#### **4.1 General Troubleshooting Steps**

- **Power Cycling**
- **Network Issues**: Inspect and replace cables as needed.
- **Ticketing**: Document issues for tracking and resolution.

#### **4.2 Node Warranty and Replacement**

- Handle nodes beyond their 5-year warranty or approaching the 7-year replacement mark.

---

### 5. Physical Troubleshooting

**Procedures for physical inspection and intervention.**

#### **5.1 Power and Component Issues**

- **Power Issues**:
    - Check status lights.
    - Reseat power connections and cables.
    - Verify power button functionality.
    - Perform a full power reset if necessary.
- **Component Replacement**: Replace or reseat components as per diagnostics.

---

### 6. Node Status Verification

**Confirm node operational status after troubleshooting.**

- **SSH and Ping Tests**: Verify connectivity.
- **IPMItool Diagnostics**: Run hardware checks if the OS is unresponsive.
