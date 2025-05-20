---
title: switch-port-vlan-identification
created: 2024-12-12
type: command-reference
system: 
tags:
  - commands
  - uiowa
  - network
  - vlan
  - switch
related_runbooks:
---
Here is a simple command reference for identifying switches, ports, and VLANs using the commands you provided:

---

### Command Reference for Switch, Port, and VLAN Identification

1. **List network interfaces with VLAN and UP status:**

```
ip a | grep "UP|VLAN"
```
- This command lists all active (UP) network interfaces and those associated with VLANs.
- The `grep "UP|VLAN"` filters out lines containing either "UP" or "VLAN".

2. **Capture VLAN tagged traffic on `ens4f0`:**

```
sudo tcpdump -nn -v -i ens4f0 -s 1500 -c 1 'ether[20:2] == 0x2000'
```
- This captures a single packet on the interface `ens4f0` and checks if the Ethernet frame has a VLAN tag (`0x2000`).
- The `-nn` option disables name resolution, and `-v` increases verbosity.

3. **Capture VLAN tagged traffic on `enp129s0f1`:**

```
sudo tcpdump -nn -v -i enp129s0f1 -s 1500 -c 1 'ether[20:2] == 0x2000'
```
- This is similar to the previous command but captures packets on `enp129s0f1`.

4. **Capture and filter traffic on `enp67s0f0np0` for VLAN-related information:**

```
sudo tcpdump -nn -v -i enp67s0f0np0 -s 1500 -c 1 'ether[20:2] == 0x2000' | grep -E "Device-ID|Port-ID|VLAN"
```
- This captures VLAN tagged traffic on `enp67s0f0np0` and filters for `Device-ID`, `Port-ID`, or `VLAN` information in the packet output.

5. **Capture and filter traffic on `ens4f0` with a specific destination MAC address:**

```
sudo tcpdump -nn -v -i ens4f0 -s 1500 ether dst 01:00:0c:cc:cc:cc | grep -E "Device-ID|Port-ID|VLAN"
```
- Captures packets on `ens4f0` with a destination MAC address `01:00:0c:cc:cc:cc` (common for CDP/LLDP devices).
- Filters output for `Device-ID`, `Port-ID`, and `VLAN` information.

---

These commands help identify switches, port information, and VLANs by analyzing network interfaces and packet captures. The use of `tcpdump` is specifically for capturing and filtering network traffic related to VLANs and other network identifiers.