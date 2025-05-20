---
title: setting-up-jumbo-frames
created: 2024-11-21
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
  - network
related_docs:
---
**Modified:** 2024-11-21

# Objective:

Enable Jumbo Frames on LSS nodes to reduce the number of packets sent and improve network efficiency.


> [!TIP] TIP
> These instructions set jumbo frames until next reboot.


---

# Pre-requisites:

- Access to LSS nodes via SSH or console.
- Ensure nodes that are marked for reboot are rebooted before starting.
- Confirm which nodes require Jumbo Frames configuration.

---

# Step-by-Step Instructions:

  
### **1. Check the System Network Configuration**
- Log in to the system via SSH or console.
- Run the following command to check the network interfaces:

```Shell
ip a

ip a | grep --color=auto -E '172.29.(4|5|6)\.'

ip a | grep --color=auto -E '172.29.(4|5|6)\.|bond|vlan|team'     
```

- Identify the relevant interfaces:
    - Look for links, VLANs, and bonds, specifically `1104` interfaces (e.g., `bond0`, `vlan.1104`).
        - 1104 range: 172.29.(4|5|6)

Some older or dedicated nodes may not have a `vlan.1104` interface. If you find such a node, mark it as done and move to the next node.

  

### **2. Configure Jumbo Frames**

- Run the following commands as needed to set the MTU to 9000 for the interfaces:

```Shell
     sudo ip link set <enp34s0f0> mtu 9000   
     sudo ip link set <enp34s0f1> mtu 9000   
     sudo ip link set <bond0> mtu 9000   
     sudo ip link set vlan.1104 mtu 9000   
     sudo ip link set <enp129s0f1> mtu 9000
```

These commands ensure Jumbo Frames (MTU 9000) are enabled on the interfaces.

  

### **3. Verify the Configuration**

- Test the Jumbo Frames configuration by pinging the configured address:

```Shell
     ping -s 8972 -M do 172.29.4.104
```

- A successful ping indicates Jumbo Frames are correctly configured. If successful, you should see replies without errors.

### **4. Troubleshooting**

- If Jumbo Frames are not configured properly, the following error message will appear:

```Shell
     ping: local error: message too long, mtu=1500
```

- This error indicates that the MTU is not set correctly. Review the steps and ensure the interfaces are set to `mtu 9000`. Re-run the commands if necessary.

### **5. Confirm MTU setting**

```Shell
ip link show | grep mtu
```

# Completion:

- After configuring and testing each node, mark it as complete and move on to the next.
- Double-check any nodes that did not have `vlan.1104` configured and ensure they are skipped.