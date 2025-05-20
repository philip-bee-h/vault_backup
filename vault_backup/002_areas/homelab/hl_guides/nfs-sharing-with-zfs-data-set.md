---
title: nfs-sharing-with-zfs-data-set
created: 2024-11-22
type: process
system: 
tags:
  - process
  - procedure
  - storage
  - homelab
  - nfs
related_docs:
---

### **Step 1: Create the Single ZFS Dataset**

1. **On the Proxmox Host:** Run the following command to create the `media` dataset in your ZFS pool:

```bash
zfs create tank/media
```

2. **Verify the Dataset:** Ensure the dataset was created:

```bash
zfs list
```

You should see an entry like:

```
NAME         USED  AVAIL  REFER  MOUNTPOINT
tank         1.23T 7.77T   96K   /tank
tank/media   500G  7.27T   96K   /tank/media
```


---

### **Step 2: Enable NFS Sharing**

1. **Share the Dataset via NFS:** Export the `tank/media` dataset over NFS:

```bash
zfs set sharenfs="rw=@192.168.10.0/24" tank/media
```

Replace `192.168.10.0/24` with your actual network's subnet (e.g., `192.168.1.0/24`).

2. **Verify the NFS Export:** Check the active NFS shares:

```bash
showmount -e <Proxmox-IP>
```

Replace `<Proxmox-IP>` with the IP address of your Proxmox host. You should see:

```
Export list for <Proxmox-IP>:
```


/tank/media 192.168.10.0/24

````

---

### **Step 3: Mount the NFS Share in Your VM**

1. **Install NFS Utilities:**
Inside your VM (e.g., Plex or ARR stack server), install the required NFS client tools:
```bash
sudo apt update && sudo apt install nfs-common -y
````

2. **Create a Mount Point:** Create a directory to mount the NFS share:

```bash
sudo mkdir -p /mnt/media
```

3. **Mount the NFS Share:** Mount the `tank/media` dataset from the Proxmox host:

```bash
sudo mount -t nfs <Proxmox-IP>:/tank/media /mnt/media
```

Replace `<Proxmox-IP>` with your Proxmox host’s IP address.

4. **Verify the Mount:** Check that the NFS share is accessible:

```bash
ls /mnt/media
```

It should be empty initially, ready for the script to create directories.


---

### **Step 4: Make the Mount Persistent**

1. **Edit `/etc/fstab`:** Open the file in a text editor:

```bash
sudo nano /etc/fstab
```

2. **Add an Entry for the NFS Share:** Add the following line:

```
<Proxmox-IP>:/tank/media /mnt/media nfs defaults 0 0
```

3. **Test the Configuration:** Apply the changes:

```bash
sudo mount -a
```

Confirm the share is still accessible:

```bash
ls /mnt/media
```


---

### **Step 5: Run the Script**

Once the NFS share is mounted in the VM, the script will automatically create the following subdirectories inside `/mnt/media`:

```
/mnt/media/tv
/mnt/media/movies
/mnt/media/downloads
/mnt/media/blackhole
```

These subdirectories correspond to:

- **`tv`**: For TV show files.
- **`movies`**: For movie files.
- **`downloads`**: For temporary downloads from the ARR stack.
- **`blackhole`**: For torrents or other placeholders.

You don’t need to manually create these directories—the script will handle it.

---

### **Step 6: Configure Your Services**

1. **In ARR Tools (Sonarr, Radarr, etc.):**

- Point the **download folder** to `/mnt/media/downloads`.
- Set the **library folders** for TV shows and movies:
- TV Shows: `/mnt/media/tv`
- Movies: `/mnt/media/movies`.
2. **In Plex:**

- Add libraries in Plex and point them to the appropriate directories:
- Movies: `/mnt/media/movies`.
- TV Shows: `/mnt/media/tv`.

---

### **Key Points**

1. **Single Dataset Simplicity:**

- All files and directories are stored within `tank/media`.
- ZFS snapshots and other management apply to the entire dataset.
2. **Future Flexibility:**

- You can later split subdirectories into individual ZFS datasets if needed, but this setup is efficient for now.
3. **Test Mounts:**

- Ensure the NFS share is correctly mounted before running the script.

---

Let me know if you’d like help refining or testing any part of this setup!