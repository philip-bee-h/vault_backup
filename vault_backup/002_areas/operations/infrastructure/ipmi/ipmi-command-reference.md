---
title: ipmi-command-reference
created: 2024-11-15
type: command-reference
system: 
tags:
  - commands
  - uiowa
related_runbooks:
---

# **IPMI Command Reference**

## **Overview**
This document provides essential `ipmitool` commands to query and manage IPMI interfaces for Ceph nodes. Use these commands for hardware monitoring, status checks, and troubleshooting.

---

### **1. Power Status**
Check the power state of the node (On/Off):
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi chassis power status
```

---

### **2. System Information**
Retrieve hardware details, such as manufacturer, model, serial number, and other FRU data:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi fru
```

---

### **3. Sensor Data**
Query current sensor readings (e.g., temperatures, fan speeds, voltages):
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi sensor
```

---

### **4. System Data Repository (SDR)**
Display all sensors and their current states in the SDR:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi sdr
```

---

### **5. System Event Log (SEL)**
View logged hardware alerts and errors:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi sel list
```

Retrieve detailed information about a specific event:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi sel get <event_id>
```

---

### **6. Network Configuration**
Display the network settings of the IPMI interface:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi lan print
```

---

### **7. Chassis Status**
Summarize the chassis status, including power state, intrusion detection, and system health:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi chassis status
```

---

### **8. Power Control**
Power cycle, reset, or shut down the node:
- **Power Cycle**:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi chassis power cycle
```
- **Reset**:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi chassis power reset
```
- **Power Off**:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi chassis power off
```

---

## **Batch Automation**

### **Automated Data Collection for Multiple Nodes**
1. **Create a Host List**:
   Save IPMI hostnames in a file:
```bash
echo "node1.ipmi" > ipmi_hosts.txt
echo "node2.ipmi" >> ipmi_hosts.txt
```

2. **Script for Bulk IPMI Queries**:
   Run commands for all hosts:
```bash
while read hostname; do
   echo "Gathering info for $hostname"
   sudo ipmitool -f /root/ipmipass -U ARGON -H $hostname chassis status
   sudo ipmitool -f /root/ipmipass -U ARGON -H $hostname fru
   sudo ipmitool -f /root/ipmipass -U ARGON -H $hostname sensor
   sudo ipmitool -f /root/ipmipass -U ARGON -H $hostname sdr
done < ipmi_hosts.txt > ceph_nodes_info.txt
```

   Output will be stored in `ceph_nodes_info.txt`.

---

## **Troubleshooting**

- **IPMI Unreachable**: Verify network connectivity and credentials.
- **BMC Reset**: Reset the Baseboard Management Controller (BMC) if the IPMI interface is unresponsive:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostname>.ipmi mc reset cold
```
- **Node Access**: Ensure IPMI is enabled and configured correctly on the node.

---

This updated document ensures comprehensive coverage of both status checks and data retrieval. Let me know if there are additional scenarios you'd like to include!