---
title: proxmox-update-issue
created: 2024-11-01
modified: 2024-11-01
type: troubleshooting
system:
  - homelab
  - proxmox
tags:
  - troubleshooting
  - proxmox
  - homelab
related_docs:
---
### **Issue:**

When updating or upgrading a Proxmox system, you encounter a `401 Unauthorized` error related to the Proxmox enterprise repositories, including Ceph-Quincy.

### **Cause:**

The enterprise repositories require a valid subscription. Without one, the system cannot access these repositories, resulting in the error.

### **Solution:**

Disable or remove the Proxmox enterprise repositories and ensure the free (non-subscription) repositories are used instead.

---

## **Steps**

### **1. Remove the Enterprise Ceph-Quincy Repository**

- Search for the enterprise repository in your configuration files:

```bash
grep -r 'enterprise.proxmox.com' /etc/apt/
```

- Edit the file that contains the Ceph-Quincy enterprise repository:

```bash
nano /etc/apt/sources.list.d/ceph.list
```

- Comment out or remove the Ceph-Quincy repository line by adding `#` at the beginning:

```bash
#deb https://enterprise.proxmox.com/debian/ceph-quincy bookworm enterprise
```


---

### **2. Comment Out the Enterprise Repository**

- Open the Proxmox enterprise repository file:

```bash
nano /etc/apt/sources.list.d/pve-enterprise.list
```

- Comment out the line referencing the enterprise repository:

```bash
#deb https://enterprise.proxmox.com/debian/pve bookworm pve-enterprise
```


---

### **3. Enable the `pve-no-subscription` Repository**

- Open or create the `pve-no-subscription` repository file:

```bash
nano /etc/apt/sources.list.d/pve-no-subscription.list
```

- Add the following line if it's not already present:

```bash
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
```


---

### **4. Update the Package List**

- Refresh your package list to apply the changes:

```bash
apt update
```

- Verify that the update completes without any errors.


---

### **5. Upgrade Packages (Optional)**

- Upgrade all packages to ensure your system is up-to-date:

```bash
apt upgrade
```


---

This process resolves the `401 Unauthorized` error and ensures Proxmox uses the free, non-subscription repository for updates. Let me know if you have any further questions!