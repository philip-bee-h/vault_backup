---
title: res-k8-util02-ip-update-ipmi
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: project
status: 
tags:
  - project
  - uiowa
  - idas
  - network
related_docs:
---

## **Project Overview**

**Issue Identified:**
- `util0-2` was unable to send IPMI commands to other IDAS nodes due to IP configuration issues.

---

## **Actions Taken**

### **1. Port Configuration Request to Ruxton**
- **Request:**
  > Could you configure the port for `rs-k8-util02` as a trunk port with VLANs 1131 and 1132 both tagged? This will allow IPMI management traffic for our setup.

- **Response from Ruxton:**
  - **Fiber Migration:**
    - The fiber for `rs-k8-util02` should be moved to the new network.
    - **Configured Ports:** `dc-lc-leaf-3@2/47-48`.
  - **Next Steps:**
    - Collaborate with Cody and Philip to move the fiber connections to the specified ports.
    - Replace the existing 4-meter jumper with a 5-meter or 6-meter cable as 6 meters are readily available.
    - Migrate the existing SFP (located in `lc-8`) concurrently with the fiber migration.

### **2. IP Address Assignment Request to Hostmaster**
- **Request:**
  > Could you please assign an IP address in the `172.30.21.0/24` range with the name `rs-k8-util02-loc.hpc.uiowa.edu`?

- **Response from Hostmaster:**
  ```plaintext
  IP: rs-k8-util02-loc.hpc.uiowa.edu, 172.30.21.99
  IPv4 Netmask: 255.255.255.0/24
  IPv4 Gateway: 172.30.21.1
  IPv4 DNS: 128.255.1.3, 128.255.64.11, 128.255.1.8
  ```


---