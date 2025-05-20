---
title: "ipmi-user-configuration-guide"
created: 2024-10-28
modified: 2024-10-28
type: process
system: infrastructure
tags:
  - ipmi
  - user-management
  - provisioning
  - infrastructure
---


## Prerequisites
- **IPMItool**: Confirm `ipmitool` is installed on the local node.

## Steps to Configure an IPMI User Locally

### Step 1: Identify IPMI Address
1. **Find the IPMI IP Address**:
```bash
sudo ipmitool lan print 1
```
   This command will display IP address, subnet mask, and network details.

### Step 2: List Current IPMI Users
1. To list existing users and check available slots:
```bash
sudo ipmitool user list 1
```
   - Identify an empty slot or User ID to modify if needed.

### Step 3: Add a New IPMI User
1. **Create the User**:
   Choose an available User ID (e.g., ID 3) and set the username:
```bash
sudo ipmitool user set name 3 idas
```

2. **Set the User’s Password**:
```bash
sudo ipmitool user set password 3 moped-bY2PW
```

### Step 4: Assign User Privileges
1. **Set Admin Privileges with full access**:
```shell
sudo ipmitool channel setaccess 1 3 callin=on link=off ipmi=on privilege=4
```

2. **Enable the User**:
```bash
sudo ipmitool user enable 3
```

### Step 5: Verify User Configuration
1. **Confirm User Setup**:
```bash
sudo ipmitool user list 1
```

2. **Test Login for New User**:
   Run a basic IPMI command to ensure the new user works:
```bash
sudo ipmitool chassis status
```

### Step 6: Securely Store Passwords
Store the IPMI user passwords securely:
1. **Password Storage**:
   - Place passwords in `/root/ipmipass` on `rs-k8-util02`.
   - Use Passwordstate for password management.
