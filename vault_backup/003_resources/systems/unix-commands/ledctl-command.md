---
type: reference
refrenceName: ledctl-command
tags: 
source: "{{source}}"
date: 2024-06-10
---

### Reference Note: Correcting Path for `ledctl` Command

> [!failure] **Original Error:**
> - Command attempted: `sudo /sbin/ledctl locate=/dev/disk/by-id/wwn-0x5000cca29191a720`
> - Error received: `-bash: sudo /sbin/ledctl locate=/dev/disk/by-id/wwn-0x5000cca29191a720: No such file or directory`

**Troubleshooting Step:**
- Used the `which` command to locate the actual path of the `ledctl` executable:
  ```bash
  which ledctl
  /usr/sbin/ledctl
  ```

**Resolution:**
- Correct command using the proper path identified by `which`:
  ```bash
  sudo /usr/sbin/ledctl locate=/dev/disk/by-id/wwn-0x5000cca29191a720
  ```

**Outcome:**
- The command executed successfully, allowing the LED for the specified drive to be located and managed.

**Note:**
- Always ensure to use the path as returned by `which` to avoid discrepancies due to system configuration changes or updates. Typing commands directly in the terminal rather than pasting can help avoid hidden character issues that may cause errors.
