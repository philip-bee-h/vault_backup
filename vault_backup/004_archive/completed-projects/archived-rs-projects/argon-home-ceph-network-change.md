---
title: argon-home-ceph-network-change
created: 2024-11-14
type: project
status: 
tags:
  - project
  - area-tag
  - uiowa
related_docs:
---
**Modified:**

## **Network Change Request: Bond Configuration for Ceph Cluster Nodes**
---

### Updates
- 2024-11-15
	- confirmed information w/ Cody
	- confirmed details needed for this network request with Ruxton
	- sent request to dc-hosting



---
**Contact Person:** Philip Haworth

**Summary:**  
This request is to configure network bonding for the `cphstr0[1-7]` nodes within the Ceph cluster. The bonding will use specific ports on different switches for separate functions: client access and replication. The change can be implemented at any time between Monday and Thursday of next week, as Canonical will not be working on the hardware during this period.

**Details:**

1. **Nodes Affected:**  
   - `cphstr01`, `cphstr02`, `cphstr03`, `cphstr04`, `cphstr05`, `cphstr06`, `cphstr07`

2. **Bond Configuration:**
   - **Client Access Bond**  
     - **Ports:** Top port in each NIC card  
     - **vlan:** 1104  
     - **Mode:** Access (not trunk)
   - **Replication Bond**  
     - **Ports:** Bottom port in each NIC card  
     - **vlan:** 1173  
     - **Purpose:** Replication for Ceph cluster nodes only

3. **Target Completion Date:**  
   - Between Monday and Thursday next week

**Requesting Party for Implementation:**  
   - DC Hosting


**Ruxton input**

- use dcim for. actual port description to help clarify 
Fiber NIC1 top left tcf0100 FP1682 21-22 (LC).
Fiber NIC2 top right tcf0128 FP1133 1-2 (LC).
Fiber NIC3 bottom left tcf0 175 FP1682 7-8 (LC)
Fiber Nic 4 bottom right tcf0101 FP1681 21-22 (LC)

---

## **Request:**


### Request Draft 1

Here’s a cleaned-up version of your request:

---

**Subject:** Request for Network Bonding Configuration on Ceph Nodes (`cphstr0[1-7]`)

Dear DC Hosting Team,

I hope this message finds you well. I’m submitting a request to configure network bonding on our Ceph nodes (`cphstr0[1-7]`) with specific VLAN assignments for client access and replication.

We’d ideally like to have this work completed by **Monday** so we can confirm its completion with the Canonical PM during our meeting on Tuesday. However, we understand your team’s workload and recognize this timeline may not be feasible. The absolute deadline for this work is **Thursday next week**.

### Configuration Details

#### VLAN 1104 (Client Access)
- **Ports:**
  - Fiber NIC1 (top left)
  - Fiber NIC4 (bottom right)
- **Bonding Mode:** Bonded on VLAN 1104  
- **Access Mode:** Set to **Access** (not Trunk)

#### VLAN 1173 (Ceph Replication)
- **Ports:**
  - Fiber NIC2 (top right)
  - Fiber NIC3 (bottom left)
- **Bonding Mode:** Bonded on VLAN 1173  
- **Purpose:** Dedicated for Ceph Replication

If you have any questions or require additional information, please don’t hesitate to reach out. We greatly appreciate your assistance with this setup.

Thank you!

Best regards,  
Philip Haworth  