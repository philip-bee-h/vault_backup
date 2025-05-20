---
title: hpc-home-dir-retention-policy
created: 2025-03-28
type: general
tags:
  - general
  - uiowa
  - idas
---
Work flow approval documentation

# HPC Home Directory Retention Policy

## Overview

To ensure efficient and sustainable use of Argon's HPC storage resources, we enforce a strict home directory cleanup policy.

- **User home directories will be permanently deleted after 9 months of login inactivity.**
    
- This process is **automatic**, applies to **all Argon users**, and is **not subject to exceptions**.
    

## Inactivity Definition

A user is considered **inactive** if they have not successfully logged in to Argon for **9 consecutive months**.

- Inactivity is determined based on **Active Directory (AD) group membership** used for HPC access.
    
- If a user is removed from the AD group that grants login access to Argon, their home directory becomes **eligible for deletion**.
    

## Deletion Process

Once a user account has been inactive for 9 months:

- The user's **entire home directory** (`/home/username`) is **permanently deleted**.
    
- This includes **all files and subdirectories**, regardless of ownership or project association.
    
- **No backups or archives** are made by the HPC team.
    

## No Notification or Recovery

- **No warning emails** will be sent.
    
- **No recovery** is possible once the data is deleted.
    
- The HPC team **cannot provide access** to home directories after removal.
    

## Responsibility and Recommendations

It is the user's responsibility to manage and back up their data:

- **Back up your data regularly**, especially before periods of inactivity such as graduation, sabbatical, or project transition.
    
- If you need to keep data long-term but won’t be using Argon actively, consider moving it to alternate storage:
    
    - **Large Scale Storage (LSS)** for persistent, research-related data.
        
    - **Departmental or PI-managed** storage solutions.
        

---

# User-Facing Policy (Onboarding/Portal)

## Home Directory Cleanup Policy

To ensure efficient use of HPC storage resources, we enforce a strict home directory cleanup policy:

> **If you have not logged in to the Argon HPC cluster for 9 months, your home directory will be permanently deleted.**

### What You Need to Know

- Inactivity is based **only on login activity**. If you don’t log in for 9 months, your data will be removed.
    
- There are **no reminder emails**, **no grace periods**, and **no backups**.
    
- Once deleted, **your files cannot be recovered**.
    

### Why This Matters

Clearing inactive home directories ensures Argon remains **fast**, **efficient**, and **available** for active research. It also helps manage **storage costs** and **sustainability**.

### Your Responsibility

- **Back up any important data** before extended absences.
    
- If you're **leaving the university** or **pausing research**, migrate your files off Argon.
    

### Need Long-Term Storage?

Argon is for **active research use only**. For long-term storage:

- Ask about our **Large Scale Storage (LSS)** service.
    
- Or speak with your **PI or department** about other storage options.
    

---



## **Implementation Recommendations for Home Directory Cleanup Policy**

To automate and enforce the home directory retention policy, consider the following steps:

---

### **1. Detect Inactive Users**

Use system tools or authentication logs to identify users who haven’t logged in for 9 months.

#### Example (using `lastlog`):

```bash
lastlog | awk '$4=="**Never" || $9 ~ /2024/ {print $1, $9}'
```

Alternatively, extract login timestamps from:

- `/var/log/wtmp` (via `last`)
    
- Central authentication logs (e.g., LDAP, SSSD, or IPA logs)
    
- SSHD logs if stored separately
    

---

### **2. Automate Cleanup**

Set up a scheduled job (e.g., via `cron` or systemd timer) to:

- Check each user’s last login date
    
- Compare it against a calculated cutoff (e.g., 9 months ago)
    
- If the threshold is exceeded, delete the user’s home directory
    

#### Example Cleanup Snippet:

```bash
user_home="/home/$username"
if [ "$last_login" -lt "$cutoff_date" ]; then
  rm -rf "$user_home"
fi
```

**Tip:** Consider logging each deletion and sending a report to admins.

---

### **3. Sync with Account Lifecycle**

Integrate home directory cleanup with your **user deprovisioning process**:

- Monitor deactivated or expired accounts centrally
    
- Start the 9-month countdown from the deactivation date
    
- Helps prevent accidental removal of accounts still in use (e.g., for automated workflows or monitoring)
    

---

Let me know if you’d like help building a full script, cron job template, or integrating with a configuration management system like Puppet or Ansible.