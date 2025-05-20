---
title: ubuntu-server-pve-vm
created: YYYY-MM-DD
type: runbook
system: vm
status: active
tags:
  - homelab
  - ubuntu
  - proxmox
  - setup
  - server-configuration
  - process
related_docs:
---



## Description
This guide outlines the basic configuration process for setting up a simple Ubuntu Server VM in Proxmox.

---

## Steps to Create an Ubuntu Server VM

### 1. **Create Virtual Machine**
   1. Navigate to **Create VM** in Proxmox.
   2. Select your desired **Node** and assign a **VM ID** and **Name**.

---

### 2. **System Configuration**
   - **Graphic card**: Select `VirtIO-GPU`
   - **Machine**: Set to `q35`
   - **BIOS**: Use `SeaBIOS`
   - **SCSI Controller**: Choose `VirtIO SCSI single`

![[ubuntu-server-pve-vm-2.png]]

---

### 3. **Disk Configuration**
   - **Bus/Device**: Select `VirtIO Block`
   - **Storage**: Choose `VM_Data` (or your preferred storage location)
   - **Disk Size**: Set to `40GB`
   - **Cache**: Leave as `Default`
![[ubuntu-server-pve-vm-3.png]]
---

### 4. **CPU Configuration**
   - **Sockets**: Set to `1`
   - **Cores**: Set to `4`
   - **Type**: Select `host`
![[ubuntu-server-pve-vm-4.png]]
---

### 5. **Memory Configuration**
   - **Memory**: Set to `4096 MiB` (which equals `4 GB`)

---

### 6. **Networking Configuration**
   - Use **default** network settings (Proxmox typically handles basic networking automatically).

---

## Finalizing the VM Configuration
1. **Review** all your settings.
2. Click **Finish** to create the VM.

---

### Additional Notes
- For EFI boot: Store **EFI data** on **local-lvm** for better performance, while the main disk is stored on **VM_Data**.
  
---

