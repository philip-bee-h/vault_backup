---
title: 2024-10-29-rs-nov-maint-day
type: meeting
attendees: 
tags:
  - meeting
  - uiowa
  - infrastructure
  - hpc
  - storage
  - idas
related_projects:
---
# Research Services Maintenance Day Planning (ITF Ports)

**Date:** October 29, 2024  

---

## 1. Migration Overview

**Migration Date:** November 20, 2024  
**Start Time:** 8:00 AM  
**End Time:** 5:00 PM  
**Location:** Data Center Migration  
**Facilitator:** [Facilitator Name]

**Objective:**  
To successfully migrate infrastructure to the new fabric with minimal downtime, ensuring all configurations are optimized and verified prior to the migration date.

---

## 2. Key Decisions

### 2.1. Switch Configurations

- **Port Channel Adjustment:**  
  Increase the number of interfaces in the port channel from 2x10G to 4x10G to enhance bandwidth and redundancy.

- **Speed Configuration:**  
  Confirmed that the MTU speed is correctly set on both new and old switches.

- **Optics Configuration:**  
  Ensure each link has matching optics. If matching optics cannot be confirmed, revert to a 2x10G configuration.

- **CA Core Switch Link:**  
  Reconnect the second link on the CA Core Switch on the day of migration after completing other configurations.

- **Upgrade Existing 10G Switch:**  
  Proceed with upgrading the existing 10G switch to a 4x10G configuration.

### 2.2. Connection Verification

- All remaining connections listed in Smartsheet have been confirmed as correct.

### 2.3. LSS Nodes Management

- Focus exclusively on LSS nodes within the ITF environment. LSS nodes in the LC environment will not be addressed during this migration window.

### 2.4. Infrastructure Nodes in BS Row

- Move infrastructure nodes in the BS Row to the new fabric during the migration. This task will not cause outages and can be performed on migration day if not completed beforehand.

### 2.5. Schedule of Events

- **Argon Switches:** Handle earlier in the migration to allow for additional attention.
- **Infrastructure Systems (BX52 Cabinet):** Can be swapped at any time, as they are not time-sensitive.

---

## 3. Action Items

| **Action Item**                                           | **Responsible Party**          | **Due Date**                |
|-----------------------------------------------------------|--------------------------------|-----------------------------|
| Finalize port configurations with DC Hosting               | Riley J. Mattice & NES         | November 13, 2024           |
| Ensure each link has matching optics                      | DC Hosting, Riley J., John C.  | November 13, 2024           |
| Reconnect second link on CA Core Switch                   | John C. Saxton & Team          | November 20, 2024 (Migration Day) |
| Conduct pre-migration configuration checks                | DC Hosting & NES               | A few days before migration |
| Move Infrastructure Nodes in BS Row to new fabric         | DC Hosting                     | Before November 20, 2024    |
| Verify functionality of flux light-up sticks              | Ruxton T. Smith & Team         | November 20, 2024           |
| Document optics matching requirements                     | LeAnn E. Dahn                  | November 13, 2024           |
| Update Smartsheet with confirmed connections              | LeAnn E. Dahn                  | November 13, 2024           |

---

## 4. Next Steps

- **Riley J. Mattice**
  - Communicate with Alexand regarding port configurations.
  - Oversee the deployment process of the finalized configurations.

- **LeAnn E. Dahn**
  - Document the optics matching requirements.
  - Update the Smartsheet with all confirmed connections and configurations.

- **John C. Saxton & Ruxton T. Smith**
  - Ensure all technical aspects related to switch configurations and reconnections are addressed.
  - Verify the functionality of flux light-up sticks and prepare contingency plans if necessary.

- **DC Hosting Team**
  - Finalize port configurations and ensure matching optics by the specified due date.
  - Conduct pre-migration configuration checks to validate all settings.

- **All Team Members**
  - Monitor progress on assigned action items.
  - Report any issues or delays promptly to prevent migration setbacks.
  - Ensure readiness for migration day activities, adhering to the schedule of events.

---

