---
title: netbox-instructions
created: 2024-11-08
type: project
status: 
tags:
  - project
  - uiowa
  - inventory
  - network
related_docs:
---
**Modified:**

https://www.youtube.com/watch?v=zT82jOUCcW4&list=PL7sEPiUbBLo_iTds-NV-9Tu05Gg2Aj8N7

## Notes

**Bulk import**

http://localhost:8001/static/docs/getting-started/populating-data/


## Instructions
### **1. Defining Your Sites**

**Sites** in NetBox represent the physical locations where your infrastructure resides. In your case, you have two sites:

- **ITF**
- **LC**

**Steps:**
- Navigate to **“Sites”** in the NetBox sidebar.
- Click **“Add”** to create each site, providing relevant details such as:
  - **Name:** ITF, LC
  - **Slug:** Unique identifier (e.g., `itf`, `lc`)
  - **Location:** Physical address or geographical details
  - **Contact Information:** Optional but useful for documentation

### **2. Organizing Services**

You have three services:

- **Argon-HPC**
- **IDAS**
- **LSS-Storage**

To manage these services effectively, you can use **Tenants** or **Tags** in NetBox. Here's how:

#### **Option A: Using Tenants**

**Tenants** are typically used to represent different customers or organizational units. If your services are distinct enough to warrant separate management, this is a good approach.

**Steps:**
- Navigate to **“Tenants”**.
- Click **“Add”** to create each tenant:
  - **Name:** Argon-HPC, IDAS, LSS-Storage
  - **Slug:** Unique identifier (e.g., `argon-hpc`)
  - **Description:** Brief overview of each service

**Assigning Tenants:**
- When adding or editing devices, virtual machines, IP addresses, etc., assign them to the appropriate tenant.

#### **Option B: Using Tags**

If the services are more of a logical grouping rather than distinct entities, **Tags** might be more flexible.

**Steps:**
- When adding or editing any object (devices, IPs, etc.), use the **“Tags”** field to assign service tags like `Argon-HPC`, `IDAS`, `LSS-Storage`.

### **3. Managing Inventory Types**

You have multiple types of inventory across services and sites. Here's how to categorize them:

#### **a. Devices and Device Types**

Define **Device Types** to represent different categories of hardware.

**Steps:**
- Navigate to **“Device Types”** under **“Devices”**.
- Click **“Add”** to create each device type:
  - **Compute Server**
  - **GPU Server**
  - **Storage Server**
  - **Head Server**
  - **IPI Switch**
  - **Finna Band Switch**

**Attributes to Define:**
- **Manufacturer:** e.g., Dell, HP, Cisco
- **Model:** Specific model number
- **U Height:** Rack units occupied
- **Part Number:** If applicable

#### **b. Devices**

Once device types are defined, you can add devices accordingly.

**Steps:**
- Navigate to **“Devices”**.
- Click **“Add”** to create each device, specifying:
  - **Name:** Unique identifier for each device
  - **Device Type:** Select from the types you defined
  - **Device Role:** Define roles like `Compute`, `GPU`, `Storage`, `Networking`
  - **Site:** ITF or LC
  - **Tenant:** Assign to Argon-HPC, IDAS, or LSS-Storage
  - **Rack:** Assign to a specific rack within the site
  - **Position and Face:** Physical location within the rack

#### **c. Racks and Rack Groups**

Organize devices within racks for better physical management.

**Steps:**
- Navigate to **“Racks”**.
- Click **“Add”** to create racks within each site:
  - **Name:** Rack identifier
  - **Site:** ITF or LC
  - **Rack Group:** Optional grouping if needed

### **4. Configuring VLANs**

You manage three VLANs: 1104, 1108, and 1132. Here's how to set them up:

#### **a. VLAN Groups (Optional)**

If you want to categorize VLANs further, you can use **VLAN Groups**.

**Steps:**
- Navigate to **“VLAN Groups”**.
- Click **“Add”** to create groups if needed (e.g., `Service VLANs`).

#### **b. VLANs**

Define each VLAN with its details.

**Steps:**
- Navigate to **“VLANs”**.
- Click **“Add”** to create each VLAN:
  - **Name:** e.g., `VLAN 1104`
  - **VID:** 1104, 1108, 1132
  - **Site:** Assign to ITF or LC (if VLANs are site-specific)
  - **Status:** Active
  - **Description:** Purpose or associated service

#### **c. Assigning VLANs to Interfaces**

Ensure that devices have the correct VLAN assignments on their interfaces.

**Steps:**
- Navigate to a **Device**, then **Interfaces**.
- Edit each interface to assign the appropriate VLAN.

### **5. Virtual LANs (VLANs)**

You mentioned managing three different VLANs (1104, 1108, 1132). Here's how to handle them:

- **Create VLANs** as outlined above.
- **Associate VLANs** with the respective devices and interfaces.
- **Use Prefixes or Naming Conventions** to indicate the associated service if needed (e.g., `Argon-HPC VLAN 1104`).

### **6. Virtualization and Clustering**

If you’re managing virtual machines or clusters, utilize NetBox’s virtualization features.

**Steps:**
- Navigate to **“Virtual Machines”** and **“Clusters”**.
- Click **“Add”** to create and associate virtual machines with physical devices.
- Define **Clusters** if services like Argon-HPC span multiple physical hosts.

### **7. IP Address Management (IPAM)**

Effectively manage IPs across your services and devices.

**Steps:**
- Navigate to **“IPAM”**.
- Define **Prefixes** and **IP Ranges** for each VLAN.
- **Assign IPs** to device interfaces, ensuring they align with their VLANs and services.

### **8. Cable Management and Connections**

Document how devices are interconnected.

**Steps:**
- Navigate to **“Cables”**.
- Click **“Add”** to create connections between device interfaces.
- Ensure accurate representation of physical and logical connections.

### **9. Documentation and Custom Fields**

Enhance your inventory with additional information.

**Steps:**
- Use **“Custom Fields”** to add service-specific details to devices, interfaces, or other objects.
- Utilize **“Notes”** and **“Comments”** for additional documentation.

### **10. User and Permission Management**

Ensure the right people have access to the right parts of NetBox.

**Steps:**
- Navigate to **“Users”** and **“Groups”**.
- Assign roles and permissions based on team responsibilities.

### **11. Automation and API Integration**

Leverage NetBox’s API for automation tasks.

**Steps:**
- Use the **REST API** to integrate with your existing tools (e.g., Ansible, Terraform).
- Automate IP assignments, device provisioning, and documentation updates.

### **12. Best Practices and Tips**

- **Consistent Naming Conventions:** Maintain uniform naming for devices, VLANs, and other objects to simplify searches and management.
- **Regular Updates:** Keep your NetBox instance updated with the latest information to serve as a reliable source of truth.
- **Backups:** Regularly back up your NetBox database to prevent data loss.
- **Training:** Ensure team members are trained on how to use NetBox effectively.

### **13. Example Structure**

Here’s a simplified example of how your data might be organized:

- **Sites:**
  - ITF
    - Racks: ITF-Rack1, ITF-Rack2
    - VLANs: 1104, 1108
  - LC
    - Racks: LC-Rack1, LC-Rack2
    - VLANs: 1132

- **Tenants/Tags:**
  - Argon-HPC
    - Devices: Compute Servers, GPU Servers
    - VLANs: 1104 (ITF), 1132 (LC)
  - IDAS
    - Devices: Head Servers, Compute Servers
    - VLANs: 1108
  - LSS-Storage
    - Devices: Storage Servers, J Bods
    - VLANs: 1104, 1132

- **Devices:**
  - ITF Site:
    - Argon-HPC Compute Server 1
    - Argon-HPC GPU Server 1
    - IPI Switch 1
    - Finna Band Switch 1
  - LC Site:
    - Argon-HPC Compute Server 2
    - IDAS Head Server 1
    - LSS-Storage Storage Server 1

### **14. Additional Resources**

- **NetBox Documentation:** [https://netbox.readthedocs.io/](https://netbox.readthedocs.io/)
- **NetBox GitHub Repository:** [https://github.com/netbox-community/netbox](https://github.com/netbox-community/netbox)
- **Community Support:** Join forums, Slack channels, or Discord servers related to NetBox for community assistance.

### **15. Next Steps**

1. **Plan Your Data Model:**
   - Outline all the devices, services, and VLANs you need to manage.
   - Decide on whether to use Tenants or Tags based on how you want to segregate services.

2. **Set Up Sites and Racks:**
   - Start by creating your sites (ITF and LC) and defining racks within them.

3. **Define Device Types and Roles:**
   - Create device types for all hardware categories you manage.

4. **Add Devices:**
   - Populate NetBox with your existing hardware, assigning them to the appropriate sites, racks, and services.

5. **Configure VLANs and IPAM:**
   - Define your VLANs and manage IP addresses accordingly.

6. **Document Connections:**
   - Map out how devices are interconnected using the cable management feature.

7. **Leverage Automation:**
   - Utilize NetBox’s API to integrate with your provisioning and configuration tools.

8. **Regular Maintenance:**
   - Keep your NetBox data up-to-date as your infrastructure evolves.

### **Feel Free to Ask Specific Questions!**

This overview should help you get started with structuring your NetBox instance. If you have specific questions or need detailed guidance on any particular aspect—such as setting up VLANs, managing device roles, integrating with other tools, or automating tasks—please feel free to ask!

