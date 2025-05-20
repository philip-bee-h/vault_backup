---
title: k8s-the-hard-way-vm-set-up
created: 2024-11-14
type: srv-config
category: 
tags:
  - service-config
  - homelab
related_docs:
---
## **System Overview**
- **OS**: pve-manager/8.4.1
- **Hostname**: Coruscant
- **IP Address**: 192.168.10.8
- **Web GUI**: 192.168.10.8:8006
- **PW**: 1Password``

## Philip's VWs

| **hostName** |    **IP**     |    **OS**    |
| :----------: | :-----------: | :----------: |
| k8s-ctrl-101 | 192.168.10.31 | ubuntu 22.04 |
| k8s-wrk-102  | 192.168.10.32 | ubuntu 22.04 |
| k8s-wrk-103  | 192.168.10.33 | ubuntu 22.04 |

- **k8s-ctrl-101** up
- **k8s-wrk-102** up
- **k8s-wrk-103** up



## Zan's VMs
user: zan
PW: K8sthehardway

| **hostName** |    **IP**     |    **OS**    |
| :----------: | :-----------: | :----------: |
| k8s-ctrl-201 | 192.168.10.41 | ubuntu 22.04 |
| k8s-wrk-202  | 192.168.10.42 | ubuntu 22.04 |
| k8s-wrk-203  | 192.168.10.43 | ubuntu 22.04 |

- **k8s-ctrl-201** up
- **k8s-wrk-202**
- **k8s-wrk-203** 

hostnamectl set-hostname

add to bashrc - export TERM=xterm

pbh@k8s-ctrl-101:~$ sudo nano /etc/netplan/01-netcfg.yaml
Error opening terminal: xterm-ghostty.
pbh@k8s-ctrl-101:~$

