---
title: ZFS and Zpool Quick Reference Guide
created: 2024-10-30
modified: 2024-10-30
type: reference
category:
  - storage
  - filesystem
tags:
  - zfs
  - zpool
  - storage-management
  - reference
  - commands
---
 dpool01 dp
# ZFS Quick Reference Command Guide

### Pool Management (`zpool` Commands)

1. **Create a New Pool**
```bash
zpool create <pool_name> <device>
```
*Example:* `zpool create mypool /dev/sda`

2. **List All Pools**
```bash
zpool list
```
Shows all pools with basic usage stats.

3. **View Pool Status**
```bash
zpool status <pool_name>
```
Check pool health, configuration, and errors.
- **More detailed view**
```shell
zpool status -v dpool01 | grep -E 'FAULTED|raidz|UNAVAIL|OFFLINE'
```


4. **Export/Import a Pool**
	- **Export**: `zpool export <pool_name>`
	- **Import**: `zpool import <pool_name>`

5. **Add/Remove a Device**
	- **Add**: `zpool add <pool_name> <device>`
	- **Remove**: `zpool remove <pool_name> <device>`

6. **Replace a Device**
```bash
zpool replace <pool_name> <old_device> <new_device>
```
   Replace failing drives without downtime.

7. **Clear Pool Errors**
```bash
zpool clear <pool_name> [device]
```
   Reset error counters after resolving issues.

8. **Destroy a Pool**
```bash
zpool destroy <pool_name>
```
   Permanently deletes the pool and all data.

9. **Monitor Pool Performance**
```bash
zpool iostat <pool_name> <interval>
```
   *Example:* `zpool iostat mypool 2` for 2-second updates.

---

### Filesystem and Snapshot Management (`zfs` Commands)

1. **Create a Filesystem**
```bash
zfs create <pool_name>/<filesystem_name>
```
   *Example:* `zfs create mypool/mydata`

2. **List Filesystems/Volumes**
```bash
zfs list
```
   Displays filesystems, volumes, and snapshots.

3. **Take a Snapshot**
```bash
zfs snapshot <pool_name>/<filesystem_name>@<snapshot_name>
```
   *Example:* `zfs snapshot mypool/mydata@daily`

4. **Create a Clone from Snapshot**
```bash
zfs clone <pool_name>/<filesystem_name>@<snapshot_name> <new_filesystem_name>
```
   *Example:* `zfs clone mypool/mydata@daily mypool/myclone`

5. **Send and Receive Dataset/Snapshot**
	- **Send**: `zfs send <pool_name>/<filesystem_name>@<snapshot_name>`
	- **Receive**: `zfs receive <pool_name>/<new_filesystem_name>`
   
   *Example:* `zfs send mypool/mydata@daily | ssh user@remotehost zfs receive remotepool/mydata`

6. **Rollback to Snapshot**
```bash
zfs rollback <pool_name>/<filesystem_name>@<snapshot_name>
```
   *Example:* `zfs rollback mypool/mydata@daily`

7. **Destroy Filesystem/Snapshot**
	- **Destroy Filesystem**: `zfs destroy <pool_name>/<filesystem_name>`
	- **Destroy Snapshot**: `zfs destroy <pool_name>/<filesystem_name>@<snapshot_name>`

---

### Example Workflow

1. **Create Pool and Filesystem**
   ```bash
   zpool create mypool /dev/sda
   zfs create mypool/mydata
   ```

2. **Daily Snapshot**
   ```bash
   zfs snapshot mypool/mydata@daily
   ```

3. **Replicate to Remote Host**
   ```bash
   zfs send mypool/mydata@daily | ssh user@remotehost zfs receive remotepool/mydata
   ```

4. **Restore from Snapshot**
   ```bash
   zfs rollback mypool/mydata@daily
   ```

5. **Delete Old Snapshot**
   ```bash
   zfs destroy mypool/mydata@old_snapshot
   ```

---

### Resources
- [ZFS Documentation](https://openzfs.github.io/openzfs-docs/)
- [ZFS Man Pages](https://man7.org/linux/man-pages/man8/zfs.8.html)

This guide offers a quick command reference for key ZFS tasks, ideal for daily operations and troubleshooting.