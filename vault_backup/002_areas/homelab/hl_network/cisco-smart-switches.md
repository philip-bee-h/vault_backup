---
title: Cisco Switch VLAN Setup for Homelab
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: runbook
system: network
status: active
tags:
  - homelab
  - network
  - runbook
---

## Guide to Setting up Cisco Switches with VLANs

### **Laptop Configuration**
- **IPv4 Address**: Set to `192.168.1.253`
- **Netmask**: Set to `255.255.255.0`

### **VLAN Configuration for Homelab**
- **Homelab VLAN**: `192.168.10.0/24`

### **Steps for VLAN Configuration**

### 1. **Router Setup**
   - **Objective**: Keep router in default mode to handle internet traffic, while avoiding VLAN management.

   - **Instructions**:
     1. Log in to your router (e.g., via IP `192.168.0.1` or `192.168.1.1`) using the admin credentials.
     2. Skip VLAN settings (IPTV/VoIP are irrelevant).
     3. Connect LAN Port 1 on the router to Port 1 on the Cisco switch as a **Trunk Port**.

   - **Explanation**: The router handles internet traffic, while VLAN segmentation is managed on the switch.

### 2. **Cisco Switch Setup**

   - **Objective**: Create VLANs and configure the switch to manage internal traffic and handle internet traffic via the router.

   #### Step 1: Access Cisco Switch
   - Connect your computer to the switch.
   - Log in through the web interface or CLI.

   #### Step 2: Create VLANs
   - Go to **VLAN Management** > **Create VLANs**.
   - Create necessary VLANs:
     - **VLAN 10**: Production Servers
     - **VLAN 20**: Test Servers

   - **Explanation**: VLANs segment the network logically, enhancing security and traffic management.

   #### Step 3: Assign VLANs to Ports
   - Assign devices to the correct VLAN via **Access Ports**:
     - **Port 2**: Assign to VLAN 10 (Production Servers)
     - **Port 3**: Assign to VLAN 20 (Test Servers)
   - Configure **Trunk Port**:
     - Connect Port 1 on the switch (trunk port) to the router. This allows all VLANs to pass traffic to the router.
     - Set **Native VLAN** to VLAN 1 (or default).

   - **Explanation**: VLAN assignment to ports allows segmented traffic. The **Trunk Port** sends all VLAN traffic to the router for internet access.

   #### Step 4: Enable Inter-VLAN Routing (if applicable)
   - If the switch supports Layer 3 routing:
     - Enable **Inter-VLAN Routing** to allow communication between VLANs.
   - If not, configure **router-on-a-stick** for inter-VLAN communication.

   - **Explanation**: Inter-VLAN routing enables devices on different VLANs to communicate.

### 3. **Testing and Validation**

   - **Objective**: Verify VLAN setup and internet access across all segments.

   - **Test VLAN Segmentation**:
     - Connect devices to respective ports.
     - Use `ping` or similar tools to confirm VLAN communication and isolation.

   - **Test Internet Access**:
     - Confirm internet access across all VLANs via the router trunk port.

   - **Test Inter-VLAN Communication** (if enabled):
     - Ensure VLAN 10 devices can communicate with VLAN 20 devices as expected.

   - **Explanation**: Testing verifies that VLAN segmentation and internet access function correctly.

### 4. **Maintenance and Monitoring**

   - **Objective**: Maintain VLAN functionality over time with monitoring and adjustments.

   - **Monitor VLAN Traffic**:
     - Enable monitoring on the Cisco switch.
     - Review logs for VLAN errors.

   - **Adjust VLANs as Needed**:
     - For new devices, configure switch ports to assign them to the correct VLAN.

   - **Explanation**: Monitoring helps maintain VLAN functionality as network needs evolve.

---

### **Summary**
- **Router**: Default configuration, handles internet access without VLAN management.
- **Switch**: Manages VLAN segmentation with specific ports for VLANs and a trunk port to the router.
- **Trunk Port**: Allows all VLANs to access the internet.
- **Inter-VLAN Routing**: Allows cross-VLAN communication if necessary.

This action plan provides a clear approach for VLAN setup and management, centralizing VLAN control on the switch while keeping router configuration simple. Testing and ongoing monitoring will help ensure optimal network performance as devices are added or requirements change. 
