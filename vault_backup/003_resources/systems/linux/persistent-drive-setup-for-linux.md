---
title: persistent-drive-setup-for-linux
created: 2024-12-05
type: process
system: 
tags:
  - process
  - procedure
  - linux
  - disk
related_docs:
---
Here’s a consolidated reference document combining all the steps for partitioning, formatting, setting up `fstab`, and ensuring persistent mounting of a drive on Linux:

---

## **Setting Up a Drive on Linux for Persistent Use**

This guide covers partitioning, formatting, and configuring a drive for automatic mounting using `/etc/fstab`.

---

### **1. Identify the Drive**

First, locate the drive you want to configure using `lsblk`:

```bash
lsblk
```

Example output:

```plaintext
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0 953.9G  0 disk 
├─sda1   8:1    0   600M  0 part /boot/efi
├─sda2   8:2    0     1G  0 part /boot
└─sda3   8:3    0 952.3G  0 part /
sdb      8:16   0 465.8G  0 disk 
```

In this example, `sdb` is the drive to configure.

---

### **2. Unmount the Drive (if Mounted)**

Before making changes, unmount any mounted partitions:

```bash
sudo umount /dev/sdb1
```

---

### **3. Partition the Drive**

Run `fdisk` to create a new partition:

```bash
sudo fdisk /dev/sdb
```

In the `fdisk` prompt:

1. **Create a New Partition Table**:
    
    - For DOS (MBR): Press `o`.
    - For GPT: Press `g`.
2. **Create a New Partition**:
    
    - Press `n` to create a new partition.
    - Accept defaults to use the entire drive.
    - Press `w` to save changes and exit.

---

### **4. Format the Partition**

Format the new partition with your desired filesystem:

- For `ext4` (Linux):

```bash
sudo mkfs.ext4 /dev/sdb1
```

- For `exFAT` (cross-platform compatibility):

```bash
sudo mkfs.exfat /dev/sdb1
```


> **Note**: If using `exFAT`, install `exfat-utils`:

```bash
sudo apt install exfat-utils
```

---

### **5. Label the Partition (Optional)**

Assign a descriptive label to the partition for easy identification:

- For `ext4`:

```bash
sudo e2label /dev/sdb1 MyDrive
```

- For `exFAT`:

```bash
sudo exfatlabel /dev/sdb1 MyDrive
```


---

### **6. Find the UUID**

To set up automatic mounting, retrieve the partition's UUID:

```bash
sudo blkid
```

Example output:

```plaintext
/dev/sdb1: UUID="8765-4321" TYPE="ext4" PARTUUID="ijkl-mnop"
```

Note the `UUID` for `/dev/sdb1`, e.g., `8765-4321`.

---

### **7. Create a Mount Point**

Create a directory where the drive will be mounted:

```bash
sudo mkdir -p /mnt/mydrive
```

---

### **8. Edit `/etc/fstab` for Persistent Mounting**

Backup the current `/etc/fstab`:

```bash
sudo cp /etc/fstab /etc/fstab.bak
```

Edit `/etc/fstab`:

```bash
sudo nano /etc/fstab
```

Add the following line at the end, replacing `<UUID>` with the actual UUID and `/mnt/mydrive` with your desired mount point:

```plaintext
UUID=<UUID>  /mnt/mydrive  <filesystem_type>  defaults  0  2
```

Examples:

- For `ext4`:

```plaintext
UUID=8765-4321  /mnt/mydrive  ext4  defaults  0  2
```

- For `exFAT`:

```plaintext
UUID=8765-4321  /mnt/mydrive  exfat  defaults  0  0
```


---

### **9. Test the Configuration**

Test the new `/etc/fstab` entry without rebooting:

```bash
sudo mount -a
```

Verify the drive is mounted:

```bash
lsblk
```

---

### **10. Check the Drive (Optional)**

Run a filesystem check before using the drive:

```bash
sudo fsck /dev/sdb1
```

---

### **11. Unmount When Needed**

To safely unmount the drive:

```bash
sudo umount /mnt/mydrive
```

---

### **Summary**

1. **Partition** the drive using `fdisk`.
2. **Format** the partition to your desired filesystem.
3. **Label** the partition (optional).
4. Retrieve the **UUID** using `blkid`.
5. Add the **UUID to `/etc/fstab`** for persistent mounting.
6. **Test and verify** the setup.

This ensures your drive is always mounted at the specified location on boot. If you want to use the drive occasionally, unmount it when not in use.
