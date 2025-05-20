---
title: "ZFS Snapshot Size Discrepancy and Analysis Guide"
created: "2025-01-31"
type: resource
category:
  - 
tags:
  - resource
  - tool-name
  - uiowa
related_docs:
---

# **ZFS Snapshot Size Discrepancy and Analysis Guide**

## **Overview**

ZFS snapshots can significantly impact a user's storage quota. This guide helps in understanding how snapshot sizes are reported, how they interact with quotas, and how to determine space savings from deletions.

---

## **Key ZFS Properties Related to Snapshots**

|**Property**|**Description**|
|---|---|
|`usedbysnapshots`|Total space consumed by all snapshots of the dataset.|
|`usedbydataset`|Space used by the active dataset itself (excluding snapshots).|
|`used`|Total space consumed by the dataset and its snapshots.|
|`written`|Space written since the last snapshot.|
|`written@snapshot`|Space written since a specific snapshot.|
|`logicalused`|Logical space used before compression and deduplication.|
|`referenced`|Space currently referenced by the dataset (active data).|

---

## **Problem Statement**

In the dataset **zdata2/home/username**, snapshots appear to consume less space than reported by `usedbysnapshots`. However, `zfs list` reports a **total snapshot usage of 9.34GB**, while summing individual snapshot sizes suggests much less. This discrepancy raises the following questions:

1. **Why does the total snapshot usage (`USEDSNAP`) not match the sum of individual snapshot sizes?**
2. **How does snapshot size impact user quotas?**
3. **How can we determine space savings from deleting snapshots?**

---

## **Understanding Snapshot Size Discrepancy**

### **Why Doesn't `USEDSNAP` Match Individual Snapshot Sizes?**

4. **Snapshots Track Differences, Not Total Data**
    
    - Each snapshot captures changes since the previous snapshot.
    - The reported **size of a snapshot** (`used` column) only shows the incremental changes since the previous snapshot.
    - However, **`usedbysnapshots`** of the dataset is the cumulative effect of all snapshots that hold references to data blocks.
5. **Blocks Shared Between Snapshots**
    
    - When a file changes, the old blocks are retained by existing snapshots.
    - Deleting a snapshot does **not** immediately free up all its space if later snapshots reference the same blocks.
6. **Zero-Size Snapshots?**
    
    - Snapshots with `0B` size may have been created when no changes occurred.
    - They consume metadata space but no actual data blocks.

---

## **How Snapshots Affect Quotas**

- **Dataset quota (`quota`) applies to the active dataset only.**
    
    - Snapshots do not count **against the quota** directly.
    - However, if the dataset fills up, it **prevents further writes**, leading to the illusion that snapshots "consume" quota.
- **Space cannot be reclaimed unless snapshots are deleted.**
    
    - If a user deletes files but snapshots reference them, no space is freed.
    - As snapshots expire, space is gradually released.

---

## **Estimating Space Savings From Snapshot Deletion**

### **Method 1: Check `written@snapshot`**

Use the `written@snapshot` property to estimate how much data would be freed if a particular snapshot were deleted.

```bash
zfs get -r -o name,value written zdata2/home/username
```

Example Output:

```
zdata2/home/username@2019-03-13-00:00:00  30.4G
zdata2/home/username@2019-03-14-00:00:00  110M
zdata2/home/username@2019-03-15-00:00:00  6.35G
```

- Deleting `2019-03-13-00:00:00` **might** free **30.4G**, depending on whether later snapshots reference the same data.

### **Method 2: Use `zfs send -nv -i` for Estimation**

Compare snapshot deltas with:

```bash
zfs send -nv -i zdata2/home/username@2019-03-13-00:00:00 zdata2/home/username@2019-03-19-12:00:00
```

Example Output:

```
estimated size is 14.4G
```

- This shows that **14.4GB of data changed between these snapshots**.

### **Method 3: Sum Up Snapshot Sizes**

```bash
zfs list -H -o used -rpt snapshot zdata2/home/username | awk '{sum+=$1} END {print sum/1024/1024 " MB"}'
```

- This provides a quick **sum of all snapshot sizes**.

---

## **Recommendations & Best Practices**

### **1. Adjust User Quotas to Account for Snapshots**

- Since snapshots do not count towards quotas but prevent space from being freed, consider **allocating extra space** in quotas to accommodate snapshots.

### **2. Implement a Snapshot Retention Policy**

- Use a script to automatically delete old snapshots when they exceed a certain age or size.
- Example command to delete snapshots older than 7 days:
    
    ```bash
    zfs destroy zdata2/home/username@2019-03-*
    ```
    

### **3. Monitor Snapshot Growth**

- Set up monitoring for `usedbysnapshots` to detect unexpected growth.
- Example:
    
    ```bash
    zfs get usedbysnapshots zdata2/home/username
    ```
    

### **4. Use `zfs list` for a More Granular View**

- To see per-snapshot usage:
    
    ```bash
    zfs list -t snapshot -o name,used,referenced -s creation zdata2/home/username
    ```
    

### **5. Consider Deduplication & Compression (If Feasible)**

- Enable compression:
    
    ```bash
    zfs set compression=lz4 zdata2/home/username
    ```
    
- Deduplication can be **memory-intensive**, so only enable it if needed:
    
    ```bash
    zfs set dedup=on zdata2/home/username
    ```
    

---

## **Summary**

- **Snapshots hold onto data blocks**, which can make their reported sizes misleading.
- **Usedbysnapshots** is higher than the sum of individual snapshots due to **block sharing**.
- **Use `written@snapshot` and `zfs send -nv -i` to estimate space savings**.
- **Increase user quotas if snapshots impact available space**.
- **Automate snapshot cleanup** to prevent excessive space usage.

