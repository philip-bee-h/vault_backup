---
title: "process-updating-classroom-instances-resources"
created: "2025-02-21"
type: process
system:
  - 
tags:
  - process
  - procedure
  - uiowa
related_docs:
---


# **Process for Updating JupyterHub Instances in Kubernetes via Jenkins**

## **Overview**

This document outlines the steps for updating and managing JupyterHub instances for class environments using **Kubernetes** and **Jenkins**. The process ensures that requested resource changes are applied correctly and instances are restarted properly.

---

## **1. Understanding the Process**

Updating resources for a JupyterHub instance involves two key steps:

1. **Rebuilding the JupyterHub instance** with the correct resources.
2. **Restarting the Hub** so the updated configuration is recognized.

Each time the instance restarts, Kubernetes automatically checks for a new version.

---

## **2. Steps to Update JupyterHub via Jenkins**

Follow these steps to apply changes through Jenkins.

### **2.1 Build the Updated Instance**

1. Open **Jenkins** and navigate to the appropriate **JupyterHub build job**.
2. Click **"Build with Parameters"**.
3. Select **Main**.
4. Set the correct **instance name**:
    - Format: `jhub-spring2025-<class_code>-<section_number>`
    - Example: `jhub-spring2025-bais-6120-0700`
5. Select the **CPU and Memory resources**:
    - Default: **8 CPUs, 32GB RAM**
6. Ignore the **Home Drive** field (it is required but not used).
7. If applicable, confirm the **Shared Class Directory Path** by running:
    
```bash
ls /path/to/shared/directory
```
    
8. Click **"Build"** to start the process.

### **2.2 What Happens During the Build**

- The process **copies the new configuration file**.
- A **slightly updated container** is pushed out.
- Cached assets allow for a **fast build**.

---

## **3. Restart the Instance in Kubernetes**

After the build completes, follow these steps in Kubernetes:

### **3.1 Check Running Pods**

Run the following command to view active pods:

```bash
kubectl -n jhub-spring2025-bais-6120-0700 get pods
```

Replace `jhub-spring2025-bais-6120-0700` with the correct namespace.

### **3.2 Delete the Old Pod**

Find and delete the existing pod to trigger a restart:

```bash
kubectl -n jhub-spring2025-bais-6120-0700 delete pod <pod-name>
```

Replace `<pod-name>` with the actual pod name.

### **3.3 Verify Restart**

Run:

```bash
kubectl -n jhub-spring2025-bais-6120-0700 get pods
```

Ensure a new pod has started.

---

## **4. Important Notes**

- **Instance Naming**:
    
    - Ensure class instance names are **unique** to avoid conflicts.
    - Use the correct format: `jhub-spring2025-<class_code>-<section_number>`.
    - This prevents overlap issues, especially between **Spring and Summer courses**.
- **Access Control**:
    
    - After restarting, **only RS staff** will have access **until automatic provisioning runs**.
    - Provisioning updates run at **the top of the hour or on the half-hour**.
- **Instance Selection**:
    
    - Use the **language dropdown** in Jenkins to select the correct environment.
    - Ensure proper **CPU and RAM allocation** based on the request.

---

## **5. Quick Reference Table**

|**Step**|**Action**|
|---|---|
|1|Open Jenkins and trigger a **Build with Parameters**|
|2|Select the correct **instance name, CPU, and RAM**|
|3|Click **Build** to start the process|
|4|Use `kubectl get pods` to verify the instance in Kubernetes|
|5|Run `kubectl delete pod <pod-name>` to restart the instance|
|6|Check that the new instance is running|
|7|Ensure provisioning updates access at the next scheduled run|
