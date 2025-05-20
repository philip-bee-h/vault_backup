---
title: idas-inventory-ipmi-config
created: 2023-10-27
modified: 2024-10-27
type: project
status: in-progress
tags:
  - project
  - inventory
  - ipmi
  - idas
  - configuration
related_docs:
---
ed
# Project Overview
This project involves updating the IDAS System Inventory to configure and document IPMI (Intelligent Platform Management Interface) across all nodes in the IDAS infrastructure. Tasks include adding IPMI IP addresses and verifying configurations.

# Objectives
1. **Update** the [IDAS System Inventory Wiki](https://uiowa.atlassian.net/wiki/spaces/hpcadmin/pages/652115979/IDAS+System+Inventory).
2. **Configure IPMI** on each node to ensure remote management and monitoring capabilities.

# Node Configuration Checklist

## Head Nodes
- [x] **rs-k8-head01.idas**
	- [x] Model: Supermicro AS-1114S-WTRT
	- [x] Serial Number: S367023X0A07632
	- [x] IPMI IP: 172.30.21.56
	- [x] Configure IPMI

- [x] **rs-k8-head02.idas**
	- [x] Model: Supermicro AS-1114S-WTRT
	- [x] Serial Number: S367023X0A07657
	- [x] IPMI IP: 172.30.21.57
	- [x] Configure IPMI

## Application/Utility Nodes
- [x] **rs-k8-app01.idas - 
	- [x] Model: Supermicro
	- [x] Serial Number: S265343X9208826
	- [x] IPMI IP: 172.30.21.44
	- [x] Configure IPMI

- [x] **rs-k8-app02.idas** - 
	- [x] Model: Supermicro SYS-1029U-TRTP2
	- [x] Serial Number: S265343X9208824
	- [x] IPMI IP: 172.30.21.43
	- [x] Configure IPMI

- [x] **rs-k8-util01**
	- [x] Model: Supermicro AS-1114S-WTRT
	- [x] Serial Number: S367023X0A07636
	- [x] IPMI IP: 172.30.21.42
	- [x] Configure IPMI

- [x] **rs-k8-util02**
	- [x] Model: Gigabyte R152-Z31-00
	- [x] Serial Number: GMG6N5412A0026
	- [x] IPMI IP: 172.30.21.8
	- [x] Configure IPMI

- [x] **rs-k8-util03**
	- [x] Model: Gigabyte R152-Z31-00
	- [x] Serial Number: GMG7N9212A0023
	- [x] IPMI IP: 172.30.21.10
	- [x] Configure IPMI

## Compute Nodes
- [x] **rs-k8-compute01.idas**
  - [x] Model: Asus ESC4000 G4
  - [x] Serial Number: K4S0GX0001KL
  - [x] IPMI IP: 172.30.21.45
  - [x] Configure IPMI

- [x] **rs-k8-compute02.idas**
  - [x] Model: Asus ESC4000 G4
  - [x] Serial Number: K4S0GX0001J8
  - [x] IPMI IP: 172.30.21.46
  - [x] Configure IPMI

- [ ] **rs-k8-compute03.idas**
  - [x] Model: Gigabyte G221-Z30-00
  - [x] Serial Number: DJ8000121A0065
  - [ ] IPMI IP ==Where is this -no ipmi==
  - [x] Configure IPMI

- [ ] **rs-k8-compute04.idas** - ==Cant ssh in - need idasadmin pw==
  - [x] Model: Gigabyte
  - [ ] Serial Number: _Not provided_
  - [ ] IPMI IP
  - [ ] Configure IPMI

- [x] **rs-k8-compute05.idas**
  - [x] Model: Gigabyte R272-Z30-00
  - [x] Serial Number: GJG6P8612A0030
  - [x] IPMI IP: 172.30.21.64
  - [x] Configure IPMI

- [x] **rs-k8-compute06.idas**
  - [x] Model: Gigabyte G292-Z44-00
  - [x] Serial Number: GKGCNA912A0018
  - [x] IPMI IP: 172.30.21.71
  - [x] Configure IPMI

- [x] **rs-k8-compute07.idas**
  - [x] Model: Gigabyte G292-Z44-00
  - [x] Serial Number: GKGCNA912A0022
  - [x] IPMI IP: 172.30.21.73
  - [x] Configure IPMI

- [ ] **rs-k8-compute08.idas**
  - [x] Model: Asus K14PP-D24 Seriesß
  - [ ] Serial Number: ==Not listed==
  - [ ] IPMI IP `sudo ipmitool lan print 1 Could not open device at /dev/ipmi0 or /dev/ipmi/0 or /dev/ipmidev/0: No such file or directory`
  - [ ] Configure IPMI

- [ ] **rs-k8-compute09.idas**
  - [x] Model: Asus RS720A-E12-RS12
  - [x] Serial Number: RCS0MD00005J
  - [ ] IPMI IP - ==Where is this -no ipmi ip==
  - [ ] Configure IPMI

- [x] **rs-k8-compute10.idas**
  - [x] Model: Asus RS720A-E12-RS12
  - [x] Serial Number: RCS0MD00005Z
  - [x] IPMI IP: 172.30.21.88
  - [x] Configure IPMI

- [ ] **rs-k8-compute11.idas**
  - [x] Model: Asus RS720A-E12-RS12
  - [x] Serial Number: RCS0MD000069
  - [ ] IPMI IP: ==Where is this -no ipmi ip==
  - [x] Configure IPMI

- [x] **rs-k8-compute12.idas**
  - [x] Model: Asus RS720A-E12-RS12
  - [x] Serial Number: RCS0MD00005U
  - [x] IPMI IP: 172.30.21.90
  - [x] Configure IPMI

- [x] **rs-k8-compute13.idas**
  - [x] Model: Asus ESC8000A-E12
  - [x] Serial Number: R9S0MD0003VB
  - [x] IPMI IP: 172.30.21.97
  - [x] Configure IPMI

- [x] **rs-k8-compute14.idas**
  - [x] Model: Asus ESC8000A-E12
  - [x] Serial Number: R9S0MD0003UB
  - [x] IPMI IP: 172.30.21.98
  - [x] Configure IPMI

## Ceph Nodes
- [ ] **rs-idas-ceph01.idas**
  - [x] Model: Asus RS500A-E11-RS12U
  - [x] Serial Number: R5S0MD00034D
  - [ ] IPMI IP
  - [ ] Configure IPMI

- [ ] **rs-idas-ceph02.idas** ==Cant ssh in - need idasadmin pw==
  - [x] Model: Asus RS500A-E11-RS12U
  - [ ] Serial Number: _Not provided_
  - [ ] IPMI IP: 
  - [ ] Configure IPMI

- [ ] **rs-idas-ceph03.idas**
  - [x] Model: Asus RS500A-E11-RS12U
  - [ ] Serial Number: R5S0MD00034H
  - [ ] K8 net IP: 192.168.64.30
  - [ ] IPMI IP: 
  - [ ] Configure IPMI

# Outstanding tasks

## Outstanding Tasks: Head Nodes

- **None** (All tasks completed)

---
## Outstanding Tasks: Application/Utility Nodes

- **None** (All tasks completed)

---
## Outstanding Tasks: Compute Nodes

1. **rs-k8-compute03.idas**
    - [ ]  IPMI IP (Missing: "Where is this - no IPMI?")
2. **rs-k8-compute04.idas**
    - [ ]  SSH Access (Cannot SSH: Need `idasadmin` password)
    - [ ]  Serial Number (Not provided)
    - [ ]  IPMI IP
    - [ ]  Configure IPMI
3. **rs-k8-compute08.idas**
    - [ ]  Serial Number (Not listed)
    - [ ]  IPMI IP (Error: `sudo ipmitool lan print 1`)
    - [ ]  Configure IPMI
4. **rs-k8-compute09.idas**
    - [ ]  IPMI IP (Missing: "Where is this - no IPMI?")
    - [ ]  Configure IPMI
5. **rs-k8-compute11.idas*
    - [ ]  IPMI IP (Missing: "Where is this - no IPMI?")

---

## Outstanding Tasks: Ceph Nodes

1. **rs-idas-ceph01.idas**
    - [ ]  IPMI IP
    - [ ]  Configure IPMI
2. **rs-idas-ceph02.idas**
    - [ ]  SSH Access (Cannot SSH: Need `idasadmin` password)
    - [ ]  Serial Number (Not provided)
    - [ ]  IPMI IP
    - [ ]  Configure IPMI

---

This structure focuses on unresolved tasks, helping you identify the work that remains across the various nodes. Let me know if you'd like further breakdowns or additional details!

# Next Steps
1. **Identify Missing IPMI IPs**: Locate missing IPMI IPs for all nodes.
2. **Verify IPMI Access**: Test and confirm access to IPMI interfaces for nodes with set IPs.
3. **Update Inventory Wiki**: Document findings and configuration details in the IDAS System Inventory Wiki.

# Notes
- Ensure each node's IPMI configuration aligns with security and access policies.
- Update any hardware or serial number discrepancies in the IDAS inventory records.


Install sudo apt ipmitool