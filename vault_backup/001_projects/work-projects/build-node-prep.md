---
title: build-node-prep
created: 2024-12-13
type: project
status: 
tags:
  - project
  - uiowa
  - idas
  - argon
  - setup
related_docs:
---
**Modified:** 2024-12-13
### Build Node Prep

**Overview:**

- **Build Server rack location:** 
	- Argon build, placed in the same rack as Argon Infra.
	- Idas build, placed in the same rack as Argon Infra.

**Rack Details:** 
_(Confirm with Ruxton)_
- 2 power and 2 fiber drops required.
    - **Copperline** for IPMI port.
    - **Bonded pair** for network.

**Tags:** 
_(Confirm with Ruxton)_
- Tag for **1104**.
- Tag for **GSA** in ITF-1182? this will come from hostmaster
- tag 1108

**Cabling:**

- Ensure cables are hung properly.

**Action Items:**

1. **Email Hostmaster** for IP and IPMI address for:
    - `itf-argon-build01`
    - `itf-idas-build01`
2. **Contact Ruxton** about fiber lines for the 3 Argon infrastructure nodes in the same rack.
	1. **QUESTION:** Are we just looking for 3 fiber lines to be added?
	2. email data center hosting




**Subject:** Request for IP Addresses

Dear Hostmaster,

I am writing to request four (4) IP addresses for the following servers:

- **itf-argon-build01**: 2 IPs
- **itf-idas-build01**: 2 IPs

For each build, the requirements are as follows:

- **Public Interface**: `1182`
    - Subnet: `128.255.83.0/24`
- **IPMI: `1183`**: Name each `.ipmi` using the format 
    - Subnet: `172.30.13.0/24`

Please let me know if additional information is needed to process this request.

Thank you for your assistance.

Best regards,  



**Subject:** Request for Hosting Setup

Hello,

Please setup with the following details:

### **Rack Requirements**

- **Copperline**: For IPMI port.
- **Bonded Pair**: For network connectivity.

### **Tagging Requirements**

- Tag for **1104**.
- Tag for **GSA** in **ITF-1182** _(to be provided by Hostmaster)_.
- Tag for **1108** on the idas build node.

Please let me know if any further details or clarifications are required.

Thank you for your assistance.

Best regards,  
[Your Name]