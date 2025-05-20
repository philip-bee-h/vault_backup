
## **HPC Home Directory Retention Policy**

### **Policy Summary**

User home directories on the HPC cluster will be **automatically deleted** after **9 months of login inactivity**, with **no prior notification** and **no data archiving**.

---

### **Policy Details**

#### **Inactivity Definition**

A user is considered inactive if:

- They have **not successfully logged in** to the HPC cluster for **9 consecutive months**
    

#### **Action Taken**

- After 9 months of inactivity, the user's **home directory will be deleted permanently**
    
- This applies to all files under the user's home directory path (e.g., `/home/username`)
    

#### **No Exceptions or Notices**

- No email notifications will be sent before deletion
    
- No backups or archives will be created before removal
    
- Recovery will not be possible after deletion
    

---

### **Implementation Recommendations**

To automate and enforce this policy reliably:

#### **1. Detect Inactive Users**

Use tools like:

```bash
lastlog | awk '$4=="**Never" || $9 ~ /2024/ {print $1, $9}'
```

Or use login timestamps from your authentication system or `/var/log/wtmp`.

#### **2. Automate Cleanup**

Schedule a script (e.g., via `cron`) to:

- Check user last login date
    
- Compare against the 9-month threshold
    
- If exceeded, delete their home directory:
    

```bash
user_home="/home/$username"
if [ "$last_login" -lt "$cutoff_date" ]; then
  rm -rf "$user_home"
fi
```

#### **3. Sync with User Account Expiry**

Ideally, coordinate this with account deprovisioning:

- If the account is disabled or removed, home dir cleanup follows after 9 months
    
- Helps avoid deleting active-but-idle accounts (e.g., automated workflows)
    

---

### **Suggested Wording for Policy Documentation**

> **Home Directory Deletion Policy**  
> User home directories on the HPC cluster will be permanently deleted **after 9 months of login inactivity**. No notifications will be sent, and no backups or archives will be retained. Users are responsible for managing and backing up their own data. Recovery is not possible once data is deleted.

---

Let me know if you want:

- A sample script for automating the process
    
- An Ansible/Puppet-compatible version
    
- Integration with LDAP or user DB for centralized tracking