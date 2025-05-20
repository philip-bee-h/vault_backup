---
title: "Untitled"
created: "2025-02-10"
type: troubleshooting
system: 
tags:
  - troubleshooting
  - uiowa
related_docs:
---


## **Warewulf: Fixing Wrong Network Interface in Use**

### **1. Identify the Wrong Interface**

If a node is using the wrong network interface, check the current network settings with:

```bash
sudo wwsh node list
```

To get detailed network settings for a specific node:

```bash
sudo wwsh node print <node_name>
```

Look for incorrect `netdev`, `IPADDR`, or other settings.

---

### **2. Remove the Wrong Interface**

To delete the incorrect interface (e.g., `eth0`):

```bash
sudo wwsh node set --netdel --netdev=eth0 <node_name>
```

Confirm the deletion by typing **"Yes"** when prompted.

---

### **3. Set the Correct Interface**

Define the correct network interface (e.g., `eth2`) with the desired settings:

```bash
sudo wwsh node set <node_name> --netdev=eth2 --mtu=9000 --ipaddr 172.29.5.240 --network=172.29.4.0 -G 172.29.6.109 -M 255.255.252.0
```

- Replace `<node_name>` with the actual node name.
- Adjust `IPADDR`, `NETWORK`, `GATEWAY`, `NETMASK`, and `MTU` as needed.

Confirm the changes by typing **"Yes"** when prompted.

---

### **4. Apply the Changes**

After making the changes, update and restart provisioning:

```bash
sudo wwsh provision set <node_name>
sudo wwsh pxe update
```

Then, reboot the node to apply the new settings:

```bash
sudo ipmitool power cycle <node_name>
```

---
