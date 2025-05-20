---
title: system-info-ipmi-sconfig
created: 2025-01-18
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
related_docs:
---
## Runbook: Inventory Collection and IPMI Tool Setup

This runbook outlines the steps to collect inventory data and configure the IPMI tool for system management.

---

### **1. System Inventory Information**

To collect basic system information:

```bash
sudo dmidecode -t system
```

**Purpose**: This command retrieves detailed information about the system, including manufacturer, product name, version, and serial number.

---

### **2. Exit Root or Current Session**

Exit the current shell or session:

```bash
exit
```

**Purpose**: To cleanly terminate elevated or unnecessary sessions.

---

### **3. Check LAN Configuration document ipmi ip**

To display the LAN configuration for IPMI channel 1:

```bash
sudo ipmitool lan print 1
```

**Purpose**: Verifies the current LAN settings such as IP address, MAC address, and other networking details.

---

### **4. Clear Terminal (Optional)**

To clear the terminal screen for better readability:

```bash
clear
```

---

### **5. List Current IPMI Users**

To display a list of users for IPMI channel 1:

```bash
sudo ipmitool user list 1
```

**Purpose**: Lists all current IPMI users, their user IDs, and privileges.

---

### **6. Create or Update a User**

To set a username for an IPMI user with ID 3:

```bash
sudo ipmitool user set name 3 idas
```

**Purpose**: Assigns or updates the username for the specified user ID.

---

### **7. Set a Password for the User**

To set a password for the same user ID:

```bash
sudo ipmitool user set password 3 moped-bY2PW
```

**Purpose**: Assigns a secure password to the user.

---

### **8. Configure User Access and Privileges**

To set user access rights on channel 1:

```bash
sudo ipmitool channel setaccess 1 3 callin=on link=off ipmi=on privilege=4
```

**Purpose**:

- Enables the ability to make calls (`callin=on`).
- Disables link authentication (`link=off`).
- Enables IPMI messaging (`ipmi=on`).
- Sets the privilege level to **Administrator (4)**.

---

### **9. Enable the User**

To enable the specified user ID:

```bash
sudo ipmitool user enable 3
```

**Purpose**: Ensures the user is active and authorized to perform IPMI operations.

---

### **10. Verify User Configuration**

To confirm that the user configuration is applied:

```bash
sudo ipmitool user list 1
```

**Purpose**: Displays the updated list of users, confirming that the user ID 3 is configured and active.

---

### **11. Exit Root or Current Session**

To exit the session after completing the configuration:

```bash
exit
```

---

### **Additional Notes**

- Ensure you have proper privileges to execute these commands.
- Replace the username (`idas`) and password (`moped-bY2PW`) with secure and unique values for production environments.
- Regularly review and update user configurations to maintain security.

---

This runbook provides a clear step-by-step guide for collecting inventory data and setting up IPMI tool configurations efficiently.


---
## One liner script to configure ipmi

**Detailed Description:** This script configures an IPMI user with the following steps:

1. Sets the username for a specific user ID using `ipmitool user set name`.
2. Assigns a password to the user ID using `ipmitool user set password`.
3. Configures the access settings for the user ID on the specified IPMI channel using `ipmitool channel setaccess`, enabling call-in and IPMI access while disabling link access, and assigning a privilege level.
4. Enables the user ID using `ipmitool user enable`.
5. Lists all users on the specified IPMI channel using `ipmitool user list` to verify the configuration.
6. Outputs a success message once the configuration is complete.

This ensures the IPMI user is fully configured, enabled, and verified for remote management.

```bash
CHANNEL=1; USER_ID=3; USERNAME="idas"; PASSWORD="moped-bY2PW"; PRIVILEGE=4; \
sudo ipmitool user set name $USER_ID $USERNAME && \
sudo ipmitool user set password $USER_ID $PASSWORD && \
sudo ipmitool channel setaccess $CHANNEL $USER_ID callin=on link=off ipmi=on privilege=$PRIVILEGE && \
sudo ipmitool user enable $USER_ID && \
sudo ipmitool user list $CHANNEL && \
echo "IPMI configuration completed successfully!"
```