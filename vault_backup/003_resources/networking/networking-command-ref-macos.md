---
title: "networking-command-ref-macos"
created: "2025-05-06"
type: command-reference
system: 
tags:
  - commands
  - uiowa
related_runbooks:
---
# **macOS Networking Command Reference**

## **1. Inspect Interfaces**

### **List Interfaces**

```bash
ifconfig
```

- Shows all active and inactive interfaces.
    

### **Show Specific Interface**

```bash
ifconfig en0
```

- Displays detailed config for a specific interface (`en0` is typically Wi-Fi).
    

---

## **2. Configure Interface IP Address (Temporary)**

> ⚠️ Temporary—resets on reboot or network change.

### **Assign IP to Interface**

```bash
sudo ifconfig en0 inet 192.168.1.10 netmask 255.255.255.0
```

### **Bring Interface Up**

```bash
sudo ifconfig en0 up
```

---

## **3. View and Modify Routing Table**

### **View Routing Table**

```bash
netstat -rn
```

or

```bash
route -n get default
```

### **Add a Route**

```bash
sudo route add -net 192.168.2.0/24 192.168.1.1
```

### **Delete a Route**

```bash
sudo route delete -net 192.168.2.0/24 192.168.1.1
```

### **Set Default Gateway**

```bash
sudo route add default 192.168.2.1
```

---

## **4. Test Connectivity**

### **Ping a Host**

```bash
ping 192.168.1.11
```

### **Traceroute**

```bash
traceroute 8.8.8.8
```

---

## **5. DNS Configuration (Temporary)**

### **Set DNS for Interface**

```bash
networksetup -setdnsservers Wi-Fi 8.8.8.8 1.1.1.1
```

### **Clear DNS Settings**

```bash
networksetup -setdnsservers Wi-Fi empty
```

---

## **6. GUI-Based Alternatives (Persistent Configuration)**

- **System Settings > Network** allows you to configure:
    
    - IP addresses (manual/DHCP)
        
    - Subnet mask
        
    - Router (gateway)
        
    - DNS settings
        
- Changes here are persistent across reboots.
    

---

## **7. Common Interface Names**

|Interface|Purpose|
|---|---|
|`en0`|Ethernet or Wi-Fi (depending on model)|
|`en1`|Additional Ethernet or Thunderbolt adapter|
|`lo0`|Loopback|

---

## **Command Summary Table**

|Task|macOS Command|
|---|---|
|List interfaces|`ifconfig`|
|Assign IP|`sudo ifconfig en0 inet 192.168.1.10 netmask 255.255.255.0`|
|Bring interface up|`sudo ifconfig en0 up`|
|View routing table|`netstat -rn`|
|Add a route|`sudo route add -net 192.168.2.0/24 192.168.1.1`|
|Add default route|`sudo route add default 192.168.2.1`|
|Ping a host|`ping 192.168.1.11`|
|Configure DNS|`networksetup -setdnsservers Wi-Fi 8.8.8.8`|

---

Would you like a comparison chart between Linux and macOS commands for study purposes?