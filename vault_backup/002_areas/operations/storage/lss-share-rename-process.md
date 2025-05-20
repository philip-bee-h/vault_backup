---
title: lss-share-rename-process
created: 2025-02-17
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
  - lss
  - puppet
related_docs:
---
# **LSS Share Rename Procedure**

### **1. Update Security Group Membership**

- Mirror the membership of the old security group to the new one.
- Email the administrator (IAM) to add users to the new group.

---

### **2. Prepare for Downtime**

- Disable Puppet on both the node and replication server:
    
```bash
# check this is the command vs `systemctl disable puppet`
puppet agent --disable "Renaming ZFS dataset"
```
    

---

### **3. Rename the ZFS Dataset**

- Run the ZFS rename command on both the primary node and replication server:
    
    ```bash
    zfs rename pool/old_dataset pool/new_dataset
    ```
- Apply necessary **znapzend** updates. **TODO**: Look up commands to do this

---

### **4. Update System Configurations**

- Modify **YAML configuration files** for:
    - **Puppet** (ZFS, Samba settings)
    - **Samba** (if applicable)

---

### **5. Deploy Configuration Changes**

- Push Puppet changes:
    
    ```bash
    git commit -am "Update ZFS and Samba configs for dataset rename"
    git push
    ```
    
- Deploy the changes:

**NOTE** This should be `puppet deploy --wait`
    ```bash
    puppet apply /path/to/manifest.pp
    ```
    ``
**NOTE**: follow-up on the node/replication with `puppet agent -tv` **Oh, also it's mentioned next**

---

### **6. Restart Services**

- Manually trigger a Puppet run on the node:
    
    ```bash
    puppet agent --test
    ```
    
- Restart the Puppet daemon:
    
    ```bash
    systemctl restart puppet
    ```
    

---

### **7. Final Cleanup**

- Email the IAM administrator to **remove the old security group**.

---

This version improves clarity, adds structure, and ensures all necessary steps are followed efficiently. Let me know if you need further refinements! 🚀