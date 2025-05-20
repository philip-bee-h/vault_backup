---
title: devops-pre-reqs
created: 2024-11-07
course:
  - devops-pre-req-course
resource: 
type: coursework
tags:
  - coursework
  - school
  - active
  - kode-kloud
---

# **Basic Networking Reference**

## **1. Key Concepts**

#### **Switches**

- Connect devices within the same local network.
- Devices must have active interfaces (physical/virtual) connected to the switch.

### **Routers**

- Connect different networks.
- Routes traffic between subnets (e.g., 192.168.1.0/24 ↔ 192.168.2.0/24).
- Act as gateways to external networks or the Internet.

### **Interfaces**

- Represent network connection points on devices (e.g., `eth0`, `ens3`).
- Must be **UP** and assigned IP addresses to communicate.

---

## **2. Inspecting and Configuring Interfaces**

### **List Interfaces**

```bash
ip link
```

- Shows interface status (e.g., UP/DOWN).
    

### **Assign IP Address**

```bash
ip addr add 192.168.1.10/24 dev eth0
```

- Applies a static IP to `eth0` (valid until reboot).
    

---

## **3. Testing Local Network Connectivity**

### **Ping a Host**

```bash
ping 192.168.1.11
```

- Verifies communication between devices in the same subnet.
    

---

## **4. Routing Basics**

### **View Routing Table**

```bash
route
```

or

```bash
ip route
```

- Displays kernel routes, gateways, and interface mappings.
    

### **Add a Route to Another Network**

```bash
ip route add 192.168.2.0/24 via 192.168.1.1
```

- Directs packets for 192.168.2.0 through gateway 192.168.1.1.
    

### **Set a Default Gateway**

```bash
ip route add default via 192.168.2.1
```

- Used for all non-local traffic, including Internet-bound packets.
    

---

## **5. Routing Table Example**

```text
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
192.168.1.0     192.168.2.1     255.255.255.0   UG    0      0        0 eth0
0.0.0.0         192.168.2.1     255.255.255.255 UG    0      0        0 eth0
```

- `0.0.0.0` = Default route (can also be written as `default`).
    
- `UG` = Up and Gateway.
    

---

## **6. Persistent Configuration**

> ⚠️ **Temporary vs Persistent**

- Changes with `ip` and `route` commands are **non-persistent**.
    
- For persistence, edit:
    
    - **Debian/Ubuntu**: `/etc/network/interfaces` or use `netplan`
        
    - **RHEL/CentOS**: `/etc/sysconfig/network-scripts/ifcfg-eth0`
        
    - **Systemd-based**: use `.network` files in `/etc/systemd/network/`
        

---

## **7. Command Summary**

|Command|Purpose|Example|
|---|---|---|
|`ip link`|List/modify interfaces|`ip link`|
|`ip addr`|Show IPs per interface|`ip addr`|
|`ip addr add`|Assign IP address|`ip addr add 192.168.1.10/24 dev eth0`|
|`route` or `ip route`|View routing table|`route`|
|`ip route add`|Add routing entry|`ip route add default via 192.168.2.1`|

---

## **8. Troubleshooting Tips**

- **Can’t reach another subnet?**
    
    - Check that a route exists via the correct gateway.
        
- **Can’t access the Internet?**
    
    - Verify a default route is set.
        
- **After reboot, settings lost?**
    
    - Ensure network configs are written to the appropriate files.
        

---

**[Kode Kloud Link](https://notes.kodekloud.com/docs/Learning-Linux-Basics-Course-Labs/Networking/Networking-Basics)**



---


