---
title: vm-blog-ubu
created: 2024-11-27
type: project
status: 
tags:
  - project
  - homelab
  - vm
  - process
related_docs:
---
  **Modified:**

# srv info
Host: Coruscant 
**IP:** 192.168.10.21
**hugo**: Doks
**framework:** Thulite



# Theme - Doks

https://getdoks.org/docs/start-here/getting-started/


# VM Notes:

### **Virtual Machine Reference**

| **Attribute**          | **Value**                                        |
| ---------------------- | ------------------------------------------------ |
| **VM ID**              | `100`                                            |
| **Name**               | `vm-blog-ubu`                                    |
| **Node**               | `coruscant`                                      |
| **Machine Type**       | `q35`                                            |
| **OS Type**            | `126` (to be updated after OS installation)      |
| **Cores**              | `2`                                              |
| **Sockets**            | `1`                                              |
| **CPU Type**           | `host`                                           |
| **Memory (RAM)**       | `2048 MiB` (2 GB)                                |
| **NUMA**               | `1`                                              |
| **Primary Disk**       | `vmdata: 15 GiB` with `iothread=on`              |
| **ISO Image**          | `local:iso/ubuntu-24.04.1-live-server-amd64.iso` |
| **Storage Controller** | `virtio-scsi-single`                             |
| **Network Interface**  | `virtio`                                         |
| **Bridge**             | `vmbr0`                                          |
| **Firewall**           | Enabled (`firewall=1`)                           |
| **QEMU Guest Agent**   | Enabled (`agent=1`)                              |
| **VGA Adapter**        | `virtio`                                         |
| **Operating System**   | _To be updated after installation_               |
| **IP Address**         | 192.168.10.21                                    |
| **SSH Setup**          | _To be configured_                               |
| **VS Code Access**     | _Pending configuration_                          |

---

### **Setup Notes**

1. **ISO Installation**:
    - Mounted ISO: `ubuntu-24.04.1-live-server-amd64.iso`.
    - Begin installation via Proxmox console.
2. **Disk and Storage**:
    - Primary disk is set to 15 GiB on the `vmdata` thin pool.
    - Ensure TRIM/Discard is configured to optimize storage usage.
3. **Memory**:
    - Configured with 2 GB of RAM (2048 MiB) for editing, site building, and SSH operations.
4. **CPU**:
    - Allocated 2 cores on a single socket using `host` CPU type for optimal performance.
5. **Networking**:
    - Connected to the `vmbr0` bridge with a `virtio` adapter.
    - Firewall is enabled for added security.

---

### **To-Do List**

- [x] **Install Ubuntu OS**  
  - Ensure minimal server setup.

- [ ] disable cloud init and set up net plan

- [ ] **Configure SSH**  
  - Install OpenSSH and test.

- [ ] **Set Up VS Code SSH Access**  
  - Use Remote-SSH extension.

- [ ] **Install Hugo, Node.js, npm**  
  - Prepare Doks environment.

- [ ] **Update Storage Settings**  
  - Verify TRIM/Discard is enabled.

- [ ] **Document IP Address and Hostname**  
  - Update this reference document.

---

This format combines compactness with detailed information in one place while still leaving room for notes and tracking setup tasks. Let me know if you prefer additional adjustments!

---

### **Example Workflow on the VM**

1. **Set up Hugo and Doks**:
    
    - Install Hugo, Node.js, and npm.
    - Clone your repository to the VM.
2. **Edit Locally**:
    
    - Make changes to your content or theme.
    - Preview your site locally using:
        
        ```bash
        npm run start
        ```
        
3. **Push Changes to Git**:
    
    - Commit and push updates to your remote repository.
4. **Cloudflare Pages Deployment**:
    
    - Cloudflare Pages will automatically deploy your updated site from your repo.

---

This setup minimizes resource usage while giving you a smooth local development experience. Let me know if you need help with the VM setup or configuring Hugo/Doks!


# Commands

### [Start the development server](https://getdoks.org/docs/start-here/installation/#start-the-development-server)

When working locally, [Hugo’s development server](https://gohugo.io/commands/hugo_server/) lets you preview your work and automatically refreshes your browser when you make changes.

Inside your project directory, run the following command to start the development server:

```bash
npm run dev
```

This will log a message to your terminal with the URL of your local preview. Open this URL to start browsing your site.

