---
title: 01-pve-prod-coruscant
created: 2024-11-14
type: srv-config
category: 
tags:
  - srv-config
related_docs:
---


## **System Overview**
- **OS**: pve-manager/8.2.7
- **Hostname**: pve-prod-coruscant
- **IP Address**: 192.168.10.7


### **SSH Connection Example**
To access the server via SSH, use the following command:

```shell
ssh root@192.168.10.7
```

---
## Hardware
- Dell PowerEdge T320

---

## **Hosted Services**

| Service Name | Port          | Configuration Reference |
| ------------ | ------------- | ----------------------- |
| (Service)    | (Port Number) | (page-link)             |


---

## **Notes**
- **2024-11-28** - Re-install due to dismantling the cluster
	- decided to remove the cluster - did not realize when I stood it up I would need to re-install.
	- had a hung up vgs - had to boot in to live iso to remove vgs
		- `vgsremove --force with something else`
		- ran a `wipefs -a /dev/sda...` on all disk
	- reinstalled proxmox
	- ran post install helper script
		- updated and dotfiles

- zfs for os and vmdata
- then zfs pool for hdd
- worth getting bigger drives and just mirroring or something like that?

---

