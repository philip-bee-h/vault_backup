---
title: IDAS Node Deployment
created: 2023-10-27
modified: 2024-10-27
type: project
status: in-progress
tags:
  - project
  - deployment
  - idas
  - network
related_docs:
---

# Project Overview
This project focuses on the deployment of new nodes for the IDAS environment, including network setup, IP address assignments, and configurations to facilitate future network migration.

# Request for Hosting
- **Network Configuration**:
  - Dual 25G NICs connected in EtherChannel with VLANs `1131`, `1104`, and `1108` tagged.
  - IPMI connection on VLAN `1131` via single copper connection.
  - **Question**: Can these nodes be directly integrated into the new network to reduce future migration efforts?

# Status Updates

## 2024-10-22
- **Delivery**: Nodes delivered to ITF.
- **Network Request**:
  - Dual 25G NICs configured in EtherChannel, VLANs `1131`, `1104`, `1108` tagged.
  - IPMI connected to VLAN `1131` (single copper).
  - Inquired if nodes can be connected to the new network for ease of future migration.
- **Hostmaster Email**:
  - Requested IPs in the `172.30.21.0/24` range for the following:
    - `rs-k8-compute13.hpc.uiowa.edu`
    - `rs-k8-compute14.hpc.uiowa.edu`
    - `k8-c13-ipmi.hpc.uiowa.edu`
    - `k8-c14-ipmi.hpc.uiowa.edu`

Hostmaster reply
```ip
IP:       rs-k8-compute13.hpc.uiowa.edu, 172.30.21.95

            rs-k8-compute14.hpc.uiowa.edu, 172.30.21.96

            k8-c13-ipmi.hpc.uiowa.edu, 172.30.21.97

            k8-c14-ipmi.hpc.uiowa.edu, 172.30.21.98

IPv4 netmask: 255.255.255.0/24

IPv4 gateway: 172.30.21.1

IPv4 DNS: 128.255.1.3, 128.255.64.11, 128.255.1.8
```
# Next Steps
1. **Await Hostmaster Response**: Confirm IP assignments for nodes and IPMI interfaces.
2. **Configure Network Interfaces**: Set up EtherChannel and VLAN tagging as requested.
3. **Finalize Node Setup**: Deploy nodes and ensure connectivity on both data and IPMI networks.

# Notes
- Ensure compatibility with future network infrastructure to streamline potential migration.
- Monitor hostmaster updates and confirm IP allocation prior to configuration.




```ip
IP: rs-k8-util02-loc.hpc.uiowa.edu, 172.30.21.99
IPv4 netmask: 255.255.255.0/24
IPv4 gateway: 172.30.21.1
IPv4 DNS: 128.255.1.3, 128.255.64.11, 128.255.1.8
```



My request to ruxton for util02

> Could you configure the port for rs-k8-util02 as a trunk port with VLANs 1131 and 1132 both tagged? This will allow IPMI management traffic for our setup.

`dc-lc-leaf-3@2/47&2/48 configured for rs-k8-util02`