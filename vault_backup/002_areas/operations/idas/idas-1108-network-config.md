---
title: idas-1108-network-config
created: 2024-11-05
type: resource
category: 
tags:
  - resource
  - tool-name
  - uiowa
related_docs:
---
**Modified:** 2024-11-05

## Network Configuration for IDAS 1108

The network setup for IDAS 1108 is organized into distinct ranges to support various components of the infrastructure:

- **Cluster Node Range:** `192.168.64.0/22`
	-  This range is designated for cluster nodes, facilitating communication and management within the cluster.
- **Kubernetes Service Range:** `192.168.68.0/22`
	- Allocated for Kubernetes services, this range ensures that service-related traffic is properly segmented and managed
- **Pod Network:** `192.168.128.0/17`
	- Dedicated to pod networking, this large range accommodates the dynamic scaling of pods within the Kubernetes environment.

## Status of IDAS 1109

The implementation of IDAS 1109 has not commenced. The project was temporarily halted following Brian's departure and remains on hold pending future developments.

---

## Notes:

- **CIDR Notation:** The `/22` and `/17` denote the subnet masks, specifying the size of each network range.
- **Kubernetes (K8s):** Refers to the open-source platform used for automating deployment, scaling, and management of containerized applications.
