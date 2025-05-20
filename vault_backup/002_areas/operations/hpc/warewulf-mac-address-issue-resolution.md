---
title: warewulf-mac-address-issue-resolution
created: 2024-12-20
type: troubleshooting
system: 
tags:
  - troubleshooting
  - uiowa
  - argon
  - warewulf
related_docs:
---
### Issue: Incorrect MAC Address Configuration in Warewulf Nodes

**Problem:** Nodes were initially added to Warewulf with the wrong MAC addresses. Although the issue was corrected, and `wwsh print node <name>` displayed the correct information, the nodes continued to be provisioned with incorrect names and IPs after rebooting.

---

### Investigation:

The incorrect information was traced to the following directory:

```
/srv/warewulf/ipxe/cfg
```

Using the `cat <mac addr>` command, the old and incorrect node information was located within this directory.

---

### Resolution:

Since `wwsh print node <name>` already showed the correct information, the PXE cache needed to be updated. The following command was used to refresh the PXE configuration:

```bash
wwsh pxe update <name>
```

**Notes:**

- The `<name>` field supports regular expressions, making it possible to update multiple nodes at once if necessary.
- After running the above command, the nodes rebooted with the correct names and IPs.

Additionally, it is recommended to restart the DHCP service to ensure all configuration changes are properly applied. Use the following command:

```bash
systemctl restart dhcpd
```

---

### Summary:

1. Identify incorrect MAC address configuration in `/srv/warewulf/ipxe/cfg`.
2. Correct the PXE cache with `wwsh pxe update <name>`.
3. Restart the DHCP service as an optional but recommended step.

On the next reboot, the nodes will boot with the corrected configuration.