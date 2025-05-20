---
title: external-ssd-for-backups
created: 2024-12-05
type: process
system: 
tags:
  - process
  - procedure
  - linux
  - backups
  - disk
related_docs:
---
## Setting Up an External SSD for Backups

This guide outlines how to prepare and configure an external SSD for removable and convenient use as a backup drive.

---

## 1. Partition and Format the Drive

### Partition the Drive

1. Open `fdisk`:

```bash
sudo fdisk /dev/sdb
```

1. Steps in `fdisk`:
    - Press `o` to create a new partition table (DOS).
    - Press `n` to create a new partition.
    - Accept the defaults to use the entire drive.
    - Press `w` to write the changes and exit.

### Format the Partition

- Format as `ext4` (Linux):

```bash
sudo mkfs.ext4 /dev/sdb1
```

- Format as `exFAT` (cross-platform compatibility, mac):

```bash
sudo mkfs.exfat /dev/sdb1
```

> [!TIP]
> **Note**: Install `exfat-utils` if formatting as `exFAT`:

```bash
sudo apt install exfat-utils
```

---

## 2. Label the Drive

Assign a descriptive label for easier identification:

- For `ext4`:

```bash
sudo e2label /dev/sdb1 BackupSSD
```

- For `exFAT`:

```bash
sudo exfatlabel /dev/sdb1 BackupSSD
```


---

## 3. Test Mounting

- Plug in the drive, and verify it mounts automatically.
- Confirm the mount with:

```bash
lsblk
```


---

## 4. Backup Workflow

### Rsync-Based Backup:

Synchronize data with `rsync`:

```bash
rsync -avh --delete /path/to/data/ /run/media/<username>/BackupSSD/
```

### Manual Backup:

Use a file manager or the `cp` command:

```bash
cp -r /path/to/data /run/media/<username>/BackupSSD/
```

---

## 5. Check Drive Health (Optional)

Run a file system check before each backup:

```bash
sudo fsck /dev/sdb1
```

---

## 6. Unmount the Drive

Unmount cleanly before unplugging:

```bash
sudo umount /run/media/<username>/BackupSSD
```

---

Your external SSD is now ready for backups and removable use. 