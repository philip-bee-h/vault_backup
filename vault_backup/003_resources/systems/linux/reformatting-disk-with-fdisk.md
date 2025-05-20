---
title: reformatting-disk-with-fdisk
created: 2024-12-05
type: process
system: 
tags:
  - process
  - procedure
  - linux
  - commands
  - disk
related_docs:
---
## Reformatting a Drive with `fdisk` on Linux

This guide outlines how to reformat a drive using `fdisk` and prepare it for use.

---

### 1. Identify the Drive

Use `lsblk` to find the drive you want to reformat:

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

In this example, `sdb` is the drive to reformat.

---

### 2. Unmount the Drive (if Mounted)

Before formatting, unmount any mounted partitions:

```bash
sudo umount /dev/sdb1
```

---

### 3. Open `fdisk`

Run `fdisk` to modify the drive:

```bash
sudo fdisk /dev/sdb
```

---

### 4. Reformat the Drive in `fdisk`

Follow these steps in the `fdisk` prompt:

1. **Create a New Partition Table**:
    
    - For DOS (MBR): Press `o`.
    - For GPT: Press `g`.
2. **Create a New Partition**:
    
    - Press `n` to create a new partition.
    - Accept the default partition number and size to use the entire disk.
    - Press `Enter` to confirm.
3. **Write Changes**:
    
    - Press `w` to save the changes and exit.

---

### 5. Format the Partition

Format the new partition with the desired filesystem:

- For `ext4`:

```bash
sudo mkfs.ext4 /dev/sdb1
```

- For `exFAT`:

```bash
sudo mkfs.exfat /dev/sdb1
```


> **Note**: Install `exfat-utils` if using `exFAT`:

```bash
sudo apt install exfat-utils
```

---

### 6. Label the Partition (Optional)

Assign a label to the partition for easier identification:

- For `ext4`:

```bash
sudo e2label /dev/sdb1 MyDrive
```

- For `exFAT`:

```bash
sudo exfatlabel /dev/sdb1 MyDrive
```


---

### 7. Mount the Drive

Mount the newly formatted partition to test:

```bash
sudo mount /dev/sdb1 /mnt
```

Verify with:

```bash
lsblk
```

---

### 8. Unmount After Use

Unmount the drive when done:

```bash
sudo umount /mnt
```

---

### Summary

Your drive is now reformatted and ready for use. Adjust the filesystem type and partitioning as needed for your specific use case.

