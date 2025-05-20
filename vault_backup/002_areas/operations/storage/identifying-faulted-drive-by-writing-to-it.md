---
title: identifying-faulted-drive-by-writing-to-it
created: 2024-10-30
modified: 2024-10-30
type: troubleshooting
system: 
tags:
  - troubleshooting
  - uiowa
  - storage
  - zfs
related_docs:
---
### Identifying Faulted or Failed Disks in ZFS Servers

When troubleshooting disks in a ZFS storage server, it may be necessary to identify which disks are faulty. One effective method is to write data to each disk, which will cause the associated activity lights to illuminate. Here's how to do it using the `dd` command.

#### Requirements

- Access to a terminal on the ZFS server.
- Sufficient permissions to write to the disks.

#### Instructions

1. **Open a Terminal:**ø[[endor-server-setup]]
2. 
   - Connect to your ZFS server via SSH or directly.

3. **Write to the Disks:**
   - Use the following command to write zeros to a file, creating multiple 1GB files:
   ```bash
   dd if=/dev/zero of=testfile bs=1G count=20
   ```
   - **Parameters:**
     - `if=/dev/zero`: This specifies the input file, which in this case is a special file that produces zeros.
     - `of=testfile`: This specifies the output file name. You can change this to any valid path where you have write permissions.
     - `bs=1G`: This sets the block size to 1GB.
     - `count=20`: This defines how many blocks to write. Adjust this number if you want to write a different total size (e.g., use `count=10` for 10GB).

3. **Monitor the Disk Activity Lights:**
   - As the command executes, monitor the activity lights on your disk drives. Identify the disk that does not light up or fails to respond.

4. **Clean Up:**
   - After identifying the faulty disk, you can delete the `testfile` created:
   ```bash
   rm testfile
   ```

#### Note:
- Be cautious when using this method, especially on production systems, as writing large files can impact performance and free space.
