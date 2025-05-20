---
title: "locating-ipmi-address-argon-nodes"
created: 2024-10-28
modified: 2024-10-28
type: reference
system: hpc
tags:
  - ipmi
  - hpc
  - argon
  - documentation
---


# Locating IPMI Address for Argon Nodes

## Description
This documentation outlines steps to locate the IPMI addresses for Argon HPC nodes from the `argon-itf-head` node.

---

## Commands / Snippets

**Locating IPMI Address**
- From `argon-itf-head`, use the following command:
```shell
grep ipmi-switch /etc/hosts
```
- **Example Output**:
```shell
[hawrth@argon-itf-head ~]$ grep ipmi-switch /etc/hosts
172.29.22.1 argon-ipmi-switch-r1
172.29.22.48 argon-ipmi-switch-r9
172.29.22.3 argon-ipmi-switch-r3
172.29.22.2 argon-ipmi-switch-r2
172.29.22.6 argon-ipmi-switch-r6
172.29.22.7 argon-ipmi-switch-r7
172.29.22.47 argon-ipmi-switch-r8
...
```


**Locating 10g Switch IP Address**
- From `argon-itf-head`, use the following command:
```shell
grep 10g-switch /etc/hosts
```
- **Example Output**:
```shell
hawrth@argon-itf-head:~$ grep 10g-switch /etc/hosts
172.29.6.54 argon-bx51-10g-switch
172.29.6.56 argon-bx53-10g-switch
172.29.6.57 argon-bx55-10g-switch
172.29.6.55 argon-bx52-10g-switch
172.29.6.52 argon-bx48-10g-switch
172.29.6.51 argon-bx47-10g-switch
172.29.6.53 argon-bx49-10g-switch
172.29.5.7 argon-ca42-10g-switch
172.29.5.1 argon-bx38-10g-switch
172.29.5.6 argon-bx44-10g-switch
172.29.5.2 argon-bx39-10g-switch
172.29.5.3 argon-bx40-10g-switch
172.29.5.4 argon-bx41-10g-switch
172.29.5.5 argon-bx42-10g-switch
172.29.5.8 argon-ca44-10g-switch
172.29.5.0 argon-bx37-10g-switch
172.29.5.239 argon-ca42-10g-switch-2
172.29.5.232 argon-cf43-10g-switch
172.29.5.233 argon-cf44-10g-switch
172.29.5.234 argon-cf45-10g-switch
172.29.5.235 argon-cf46-10g-switch
172.29.5.236 argon-cf47-10g-switch
172.29.5.237 argon-cf48-10g-switch
172.29.5.238 argon-cf50-10g-switch
```


**Set Static IP on Local Host**
- See the guide for [[setting-static-ip-macos]].
- **Netmask**: `255.255.252.0`



mkty70jA98Z!g




http://argon-ca42-10g-switch-2