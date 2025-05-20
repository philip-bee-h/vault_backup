---
title: impi-troubleshooting-commands
created: 2025-02-22
type: command-reference
system: 
tags:
  - commands
  - ipmi
  - troubleshooting
related_runbooks:
---
When troubleshooting dead nodes using **IPMI**, you should consider several commands to gather useful diagnostic information. Below are some key **IPMI commands** to try:

---

## **1. Check System Event Log (SEL)**
The SEL logs may provide hardware or power-related errors that could explain why a node is unresponsive.

```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi sel list
```

To get detailed information about events:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi sel elist
```

To check how much storage is used in the SEL:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi sel info
```

To clear SEL logs if needed:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi sel clear
```

---

## **2. Check Chassis & Power Status**
To see if the node is actually powered off or stuck:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi chassis power status
```

To turn it on:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi chassis power on
```

To forcefully power off (use with caution):
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi chassis power off
```

To reset instead of cycling:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi chassis power reset
```

---

## **3. Check Sensor Data for Hardware Issues**
View temperature, fan speeds, voltage, and other sensor readings:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi sdr list
```
For a detailed output:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi sdr elist full
```

---

## **4. Check BMC (Baseboard Management Controller) Status**
BMC issues can sometimes cause a node to become unresponsive.

Check BMC status:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi mc info
```

Reset the BMC if needed:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi mc reset cold
```

---

## **5. Check Boot Progress & PXE Boot Issues**
If a node is stuck in boot:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi chassis bootparam get 5
```
To force a PXE boot for remote recovery:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi chassis bootdev pxe
```

---

## **6. Identify and Reboot a Stuck Node**
If the node is stuck or appears unresponsive:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi chassis power cycle
```
If the above fails, try a **hard reset**:
```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H <hostName>.ipmi mc reset cold
```

---

## **7. Network & Authentication Debugging**
If IPMI commands are failing or unresponsive:
- **Check if the IPMI interface is reachable:**
```bash
ping -c 4 <hostName>.ipmi
```
- **Test authentication and session issues:**
```bash
sudo ipmitool -I lanplus -f /root/ipmipass -U ARGON -H <hostName>.ipmi chassis power status
```
- **Reset the BMC network settings if needed:**
```bash
sudo ipmitool -H <hostName>.ipmi -U ARGON -f /root/ipmipass lan print 1
```

---

### **Next Steps Based on Findings**
1. If **power status shows OFF**, try turning it back on.
2. If **SEL logs show errors**, analyze the messages for hardware or PSU issues.
3. If **sensors report high temperatures**, check cooling.
4. If **network issues persist**, verify the BMC’s network config.
5. If **IPMI is unresponsive**, attempt a BMC reset (`mc reset cold`).

