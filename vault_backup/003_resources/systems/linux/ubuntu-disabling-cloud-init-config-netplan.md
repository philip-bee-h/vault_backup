---
title: ubuntu-disabling-cloud-init-config-netplan
created: 2025-01-11
type: process
system: 
tags:
  - process
  - procedure
  - ubuntu
  - networking
related_docs:
---

### **Runbook: Disabling Cloud-Init and Configuring Netplan**

#### **Step 1: Disable Cloud-Init Network Management**

Run the following command to disable cloud-init's network management:

```bash
echo "network: {config: disabled}" | sudo tee /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
```

#### **Step 2: Disable Cloud-Init Completely (Optional)**

To fully disable cloud-init from managing the system, create the following marker file:

```bash
sudo touch /etc/cloud/cloud-init.disabled
```

#### **Step 3: Move Cloud-Init's Netplan Configuration File**

If `cloud-init` has already created a network configuration file, move and rename it for manual management:

```bash
sudo mv /etc/netplan/50-cloud-init.yaml /etc/netplan/01-netcfg.yaml
```

#### **Step 4: Edit the Netplan Configuration**

Modify the netplan configuration to set up your static or dynamic IP:

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

Example for a static IP:

```yaml
network:
  version: 2
  ethernets:
    eno1:
      addresses:
        - 192.168.20.15/24
      gateway4: 192.168.20.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```

Save and exit.

#### **Step 5: Apply the Netplan Configuration**

Apply your changes to the network:

```bash
sudo netplan apply
```

#### **Step 6: Verify Configuration Persists**

Reboot the server to verify that the configuration persists:

```bash
sudo reboot
```

After reboot, confirm:

1. Cloud-init is no longer controlling networking:
    
    ```bash
    cloud-init status
    ```
    
    It should indicate "disabled."
    
2. Netplan is correctly applied:
    
    ```bash
    ip addr show
    ```
    

---

### **Summary of Commands**

```bash
echo "network: {config: disabled}" | sudo tee /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
sudo touch /etc/cloud/cloud-init.disabled
sudo mv /etc/netplan/50-cloud-init.yaml /etc/netplan/01-netcfg.yaml
sudo nano /etc/netplan/01-netcfg.yaml
sudo netplan apply
sudo reboot
```

This ensures that cloud-init no longer manages networking, and your custom netplan configuration remains persistent.



---

## **Script to disable cloud-init**

```sh
#!/bin/bash

# Script to disable cloud-init and configure netplan

echo "Disabling cloud-init network management..."
echo "network: {config: disabled}" | sudo tee /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg

echo "Fully disabling cloud-init..."
sudo touch /etc/cloud/cloud-init.disabled

echo "Checking for existing netplan configuration..."
if [ -f /etc/netplan/50-cloud-init.yaml ]; then
    echo "Moving cloud-init netplan file to manual management..."
    sudo mv /etc/netplan/50-cloud-init.yaml /etc/netplan/01-netcfg.yaml
else
    echo "No cloud-init netplan file found."
fi

echo "Applying netplan configuration..."
sudo netplan apply

echo "Done. Please verify the configuration and reboot if necessary."
```