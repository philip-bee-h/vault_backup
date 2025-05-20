---
title: lss-snapshot-retention-policy
created: 2025-01-07
type: resource
category: 
tags:
  - resource
  - tool-name
  - uiowa
  - lss
  - storage
  - zfs
related_docs:
---

# DRAFT 1

### Snapshot and Retention Options Overview

We offer three snapshot and retention options to meet different user needs for data protection and storage management. Each option determines how often snapshots are taken and how long they are retained. Since snapshots count toward your storage quota, selecting the right option is crucial for balancing data recovery needs and storage usage.

---

### **1. Standard Option**

**Description**: The default setup for most users, offering a balance between data recovery options and storage efficiency.

- **Frequency**:
    - Hourly snapshots retained for 12 hours.
    - Daily snapshots retained for 7 days.
    - Monthly snapshots retained for 3 months.
- **Considerations**:
    - Provides sufficient recovery points for most use cases.
    - Retaining monthly snapshots ensures a medium-term history for data recovery.
    - Efficient for general-purpose usage but could impact storage quotas for large datasets.
- **Ramifications**:
    - Snapshots consume storage quota, so users with large or frequently changing datasets should monitor usage.
    - Ideal for general research or operational environments requiring reliable data protection.

---

### **2. Minimal Option**

**Description**: A lightweight option for users who need basic recovery capabilities with reduced storage impact.

- **Frequency**:
    - Hourly snapshots retained for 6 hours.
    - Daily snapshots retained for 3 days.
    - Monthly snapshots retained for 1 month.
- **Considerations**:
    - Reduces snapshot-related storage usage while maintaining essential recovery options.
    - Suitable for smaller projects or less critical data where long-term recovery isn’t a priority.
- **Ramifications**:
    - Faster storage usage reduction, but fewer recovery points may limit rollback options in case of errors.
    - Best for users with storage constraints or low data volatility.

---

### **3. No Snap (Scratch) Option**

**Description**: For temporary or disposable workloads, this option disables snapshots entirely.

- **Frequency**:
    - No snapshots are created or retained.
- **Considerations**:
    - Eliminates snapshot-related storage usage.
    - Ideal for scratch or temporary data that doesn’t require recovery points.
- **Ramifications**:
    - No ability to recover data once deleted or modified.
    - Should only be used for non-critical data or temporary projects.

---

### Key Points to Remember

- Snapshots count toward your storage quota, so choosing the appropriate option can help manage storage efficiently.
- **Standard Option** is best for users needing robust recovery options.
- **Minimal Option** is tailored for smaller datasets or constrained environments.
- **No Snap Option** is a zero-overhead solution for temporary or non-critical data.

By understanding the trade-offs of each option, you can align your snapshot policy with your project needs and quota limitations.



### 1. **Standard Option**

This is the default setup balancing recovery capabilities and storage efficiency.

|Snapshot Type|Frequency|Retention Duration|
|---|---|---|
|**Frequent Snaps**|Every 15 minutes|**0 hours** (disabled)|
|**Hourly Snaps**|Every 1 hour|**12 hours**|
|**Daily Snaps**|Every 24 hours|**7 days**|
|**Weekly Snaps**|Every 7 days|**0 weeks** (disabled)|
|**Monthly Snaps**|Every 4 weeks|**3 months**|

---

### 2. **Minimal Option**

Designed for minimal storage usage while maintaining basic recovery options.

|Snapshot Type|Frequency|Retention Duration|
|---|---|---|
|**Frequent Snaps**|Every 15 minutes|**0 hours** (disabled)|
|**Hourly Snaps**|Every 1 hour|**6 hours**|
|**Daily Snaps**|Every 24 hours|**3 days**|
|**Weekly Snaps**|Every 7 days|**0 weeks** (disabled)|
|**Monthly Snaps**|Every 4 weeks|**1 month**|

---

### 3. **No Snap (Scratch) Option**

For scratch or temporary workloads, snapshots are entirely disabled.

|Snapshot Type|Frequency|Retention Duration|
|---|---|---|
|**Frequent Snaps**|Disabled|**0 hours**|
|**Hourly Snaps**|Disabled|**0 hours**|
|**Daily Snaps**|Disabled|**0 days**|
|**Weekly Snaps**|Disabled|**0 weeks**|
|**Monthly Snaps**|Disabled|**0 months**|

---

### Summary

- **Standard**: Balanced retention for regular use cases.
- **Minimal**: Lighter retention for reduced storage needs.
- **Scratch (No Snap)**: No snapshots for disposable or temporary workloads.

Let me know if you’d like any changes or further details!


# DRAFT 2

### Snapshot and Retention Options

We offer three snapshot and retention options to meet different user needs for data protection and storage management. Each option determines how often snapshots are taken and how long they are retained. Since snapshots count toward your storage quota, selecting the right option is crucial for balancing data recovery needs and storage usage.

---

### Standard Option

**Description**: The default setup for most users, offering a balance between data recovery options and storage efficiency.

- **Frequency**:
    - Hourly snapshots retained for 12 hours.
    - Daily snapshots retained for 7 days.
    - Monthly snapshots retained for 3 months.
- **Considerations**:
    - Provides sufficient recovery points for most use cases.
    - Retaining monthly snapshots ensures a medium-term history for data recovery.
    - Efficient for general-purpose usage but could impact storage quotas for large datasets.
- **Ramifications**:
    - Snapshots consume storage quota, so users with large or frequently changing datasets should monitor usage.
    - Ideal for general research or operational environments requiring reliable data protection.

#### Snapshot Retention Table (Standard Option)

|Snapshot Type|Frequency|Retention Duration|
|---|---|---|
|**Frequent Snaps**|Every 15 minutes|**0 hours** (disabled)|
|**Hourly Snaps**|Every 1 hour|**12 hours**|
|**Daily Snaps**|Every 24 hours|**7 days**|
|**Weekly Snaps**|Every 7 days|**0 weeks** (disabled)|
|**Monthly Snaps**|Every 4 weeks|**3 months**|

---

### Minimal Option

**Description**: A lightweight option for users who need basic recovery capabilities with reduced storage impact.

- **Frequency**:
    - Every 8 hours retained for 24 hours.
    - Daily snapshots retained for 3 days.
    - Monthly snapshots retained for 1 month.
- **Considerations**:
    - Reduces snapshot-related storage usage while maintaining essential recovery options.
    - Suitable for smaller projects or less critical data where long-term recovery isn’t a priority.
- **Ramifications**:
    - Faster storage usage reduction, but fewer recovery points may limit rollback options in case of errors.
    - Best for users with storage constraints or low data volatility.

#### Snapshot Retention Table (Minimal Option)

|Snapshot Type|Frequency|Retention Duration|
|---|---|---|
|**Frequent Snaps**|Every 15 minutes|**0 hours** (disabled)|
|**Hourly Snaps**|Every 8 hours|**24 hours**|
|**Daily Snaps**|Every 24 hours|**3 days**|
|**Weekly Snaps**|Every 7 days|**0 weeks** (disabled)|
|**Monthly Snaps**|Every 4 weeks|**1 month**|

---

### No Snap (Scratch) Option

**Description**: For temporary or disposable workloads, snapshots are entirely disabled.

- **Frequency**:
    - No snapshots are created or retained.
- **Considerations**:
    - Eliminates snapshot-related storage usage.
    - Ideal for scratch or temporary data that doesn’t require recovery points.
- **Ramifications**:
    - No ability to recover data once deleted or modified.
    - Should only be used for non-critical data or temporary projects.

#### Snapshot Retention Table (No Snap Option)

|Snapshot Type|Frequency|Retention Duration|
|---|---|---|
|**Frequent Snaps**|Disabled|**0 hours**|
|**Hourly Snaps**|Disabled|**0 hours**|
|**Daily Snaps**|Disabled|**0 days**|
|**Weekly Snaps**|Disabled|**0 weeks**|
|**Monthly Snaps**|Disabled|**0 months**|

---

### Summary

- **Standard Option**: Balanced retention for regular use cases.
- **Minimal Option**: Lighter retention for reduced storage needs.
- **No Snap (Scratch) Option**: No snapshots for disposable or temporary workloads.

By understanding the trade-offs of each option, you can align your snapshot policy with your project needs and quota limitations.