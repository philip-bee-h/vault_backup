---
title: vm-samba-ubu
created: 2024-12-07
type: project
status: 
tags:
  - project
  - proxmox
  - homelab
  - samba
  - linux
related_docs:
---
**Modified:**

```sh
ssh pbh@192.168.10.23
```

![[vm-samba-ubu-1.png]]


found date out of sync and had to set it

```sh
pbh@vm-samba-ubu:~$ date
Sat Dec  7 06:05:24 AM UTC 2024
pbh@vm-samba-ubu:~$ sudo timedatectl set-timezone America/Chicago
pbh@vm-samba-ubu:~$ date
Sat Dec  7 12:05:40 AM CST 2024
```


Ran updates, installed tools, setup dotfiles

---

# 2024-11-17

- added hard disk for dataset 
	- select vm > hardware > add
![[vm-samba-ubu-2.png]]

## Samba Setup Preparation - Server Tracking Notes

#### **Environment Information**

- **Server Name:** `vm-samba-ubu`
- **Disk Information:**
    - `/dev/sda` - 40GB, mounted as `/`
    - `/dev/vda` - 4.5TB, used for media storage

#### **Steps Performed**

1. **Verify Samba Installation:**

```bash
whereis samba
```

- Output confirms Samba binary and configuration paths.

1. **Check Disks and Partitions:**

```bash
lsblk
sudo fdisk -l
```

- Identified `vda` as the target disk for media storage.

1. **Partitioning the Disk (`/dev/vda`):**

- Used `fdisk` to create a new **GPT partition**:

```bash
sudo fdisk /dev/vda
```

Steps:
	- Created a new GPT disklabel.
	- Added a single partition (`vda1`) spanning the entire disk (4.5TB).
	- Saved and exited.
1. **Formatting the Partition:**

- Formatted `vda1` as `ext4`:

```bash
sudo mkfs.ext4 /dev/vda1
```

5. **Mounting the Partition:**

- Created a directory for mounting:

```bash
sudo mkdir -p /srv/media
```

- Mounted the partition:

```bash
sudo mount /dev/vda1 /srv/media
```

- Verified the mount:

```bash
df -h | grep /srv/media
```

6. **Persistent Mount Configuration:**

- Retrieved the UUID of the partition:

```bash
sudo blkid /dev/vda1
```

Example Output:

```
UUID="b0842866-b465-4f86-bc1a-b095f0480395"
```

- Edited `/etc/fstab` to include:

```
UUID=b0842866-b465-4f86-bc1a-b095f0480395 /srv/media ext4 defaults 0 2
```

- Applied the configuration:

```bash
sudo mount -a
```

7. **Verification:**

- Verified the partition is mounted:

```bash
df -h | grep /srv/media
```

- Created and removed a test file to confirm write access:

```bash
sudo touch /srv/media/testfile
sudo rm /srv/media/testfile
```

- Checked directory contents:

```bash
ls -l /srv/media
```
#### **Post-Setup Notes**

- The disk is now mounted persistently at `/srv/media` with `ext4` filesystem.
- Ready for Samba configuration to share `/srv/media`.

## **Steps to Create a Passwordless Samba Share**

### **1. Verify the Shared Directory**

1. Ensure the directory you want to share exists (you’ve already set this up as `/srv/media`):

```bash
ls -ld /srv/media
```

If the directory does not exist, create it:

```bash
sudo mkdir -p /srv/media
```

1. Set permissions for open access:

```bash
sudo chmod -R 777 /srv/media
```

- **Note**: `777` grants full read, write, and execute permissions for all users. This is suitable for a trusted homelab but not recommended in a production environment.

---

### **2. Configure Samba for Guest Access**

Edit the Samba configuration file:

```bash
sudo nano /etc/samba/smb.conf
```

Add the following section at the bottom of the file:

```ini
[MediaShare]
    comment = Media Share for Plex and Ripping
    path = /srv/media
    browseable = yes
    read only = no
    guest ok = yes
    create mask = 0777
    directory mask = 0777
    force user = nobody
```

**Explanation of Key Parameters:**

- **`guest ok = yes`**: Allows unauthenticated (guest) access.
- **`read only = no`**: Enables write access.
- **`force user = nobody`**: Ensures all created files are owned by the `nobody` user for consistency.
- **`create mask` and `directory mask`**: Ensures new files and directories have full permissions.

Save and exit the editor (`Ctrl + O`, `Enter`, then `Ctrl + X`).

---

### **3. Restart the Samba Service**

To apply the new configuration:

```bash
sudo systemctl restart smbd
```

---

### **4. Update Firewall Rules**

Allow Samba traffic through your firewall:

```bash
sudo ufw allow samba
```

Verify the rule is active:

```bash
sudo ufw status
```

---

### **5. Mount the Share on Your Media Ripping Station and Plex Server**

On both systems, mount the Samba share with the following command:

```bash
sudo mount -t cifs //<server-ip>/MediaShare /mnt/media -o guest,uid=1000,gid=1000
```

Replace:

- `<server-ip>`: The IP address of your Samba server.
- `/mnt/media`: The local mount point on your client machine.

If the mount point (`/mnt/media`) does not exist on the client machine, create it:

```bash
sudo mkdir -p /mnt/media
```

---

### **6. Make the Mount Persistent (Optional)**

To mount the share automatically at boot on your ripping station and Plex server, add this line to `/etc/fstab` on each client machine:

```plaintext
//<server-ip>/MediaShare /mnt/media cifs guest,uid=1000,gid=1000 0 0
```

Test the `/etc/fstab` configuration:

```bash
sudo mount -a
```

---

### **Security Note**

This setup allows anyone on your local network to access the share without a password. While convenient for a homelab, it may expose your data if your network is not properly secured. Consider implementing authentication if you expand your network or require tighter controls.

---

This setup will allow your media ripping station and Plex server to access the share without any issues. Let me know if you need further assistance! 🚀