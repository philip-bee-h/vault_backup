---
title: update-idas-nics-for-vlans
created: 2024-12-12
type: general
tags:
  - general
  - uiowa
  - idas
---
'**Modified:**

**IDAS Update Task**

- **Objective**: Update IDAS to ensure both ports get all VLANs.
- **Nodes Involved**: All nodes except the new compute nodes.
- **Configuration Method**: Use Puppet for configuration.
- **Action Items**:
    - Ruxton to leave the copper port active for Puppet to run.
    - Remove the copper port after Puppet configuration.
    - Discuss with Ruxton about the need for a non-tagged port for auto provisioning.