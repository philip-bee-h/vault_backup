---
title: ubu-blog-pve-install
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: resource
category:
  - system
tags:
  - homelab
  - ubuntu
  - proxmox
  - blog-server
  - active
---

# Ubuntu Blog VM Configuration

## Details
- **VM ID**: 238
- **Name**: ubu-blog
- **Static IP**: `192.168.0.238`

## Setup Notes
- **Operating System**: Ubuntu Server 24.04
- **Disk Size**: 40GB
- **CPU**:
  - **Sockets**: 1
  - **Cores**: 4
  - **Type**: Host
- **Memory**: 4096 MiB (4 GB), expandable to 8 GB if needed

## Network Configuration
- **Subnet**: `192.168.0.0/24`
- **Static IP Address**: `192.168.0.238` (ensure DHCP settings exclude this address)
- **Gateway**: `192.168.0.1`
- **Name Servers**: `8.8.8.8`, `1.1.1.1`

---

This version is organized for quick reference, with each section clearly separated for readability. Let me know if you need any additional modifications!