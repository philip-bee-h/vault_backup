---
title: sge-showing-incorrect-cpu-available
created: 2025-01-31
type: troubleshooting
system: 
tags:
  - troubleshooting
  - uiowa
  - argon
  - hpc
related_docs:
---
# **Issue**

## **Symptoms**

- The compute node reports an incorrect number of CPUs in Grid Engine.
- Example of incorrect output (`qhost` shows only 64 CPUs when 128 should be available):
    
```
❯ qhost -q -h argon-itf-ca44-06
HOSTNAME                ARCH         NCPU NSOC NCOR NTHR  LOAD  MEMTOT  MEMUSE  SWAPTO  SWAPUS
----------------------------------------------------------------------------------------------
global                  -               -    -    -    -     -       -       -       -       -
argon-itf-ca44-06       lx-amd64       64    2   64   64     -  251.6G       -     0.0       -
   all.q                BIPC  0/20/64       aAu
   RON                  BIPC  0/0/64        aAu
```
    
- The issue needs to be fixed so that the correct number of CPUs (128) is reported.
- Example of correct output after fixing:
    
```
❯ qhost -q -h argon-itf-ca44-06
HOSTNAME                ARCH         NCPU NSOC NCOR NTHR  LOAD  MEMTOT  MEMUSE  SWAPTO  SWAPUS
----------------------------------------------------------------------------------------------
global                  -               -    -    -    -     -       -       -       -       -
argon-itf-ca44-06       lx-amd64      128    2   64  128 60.37  251.6G    9.8G    2.0G     0.0
   all.q                BIPC  0/128/128
   RON                  BIPC  0/0/128
```
    

---

# **Fix**

### ✅ **Step 1: Stop & Restart Execution Daemon Manually**

Since `systemctl` is not managing `sge_execd`, stop it manually:

#### **On the Compute Node**

```bash
sudo pkill -f sge_execd
```

- This force-stops the execution daemon.

Restart it manually:

```bash
/opt/sge/bin/lx-amd64/sge_execd
```

- This ensures the daemon is restarted and running.

Verify that it is active:

```bash
ps aux | grep sge_execd
```

---

### ✅ **Step 2: Verify CPU Load Reporting**

After restarting the daemon, check if Grid Engine now recognizes all CPUs.

#### **On the Head Node**

```bash
qhost  -q -h <node-name>
```

- Ensure `NCPU` matches the expected number of CPUs.
- if it does verify user access

#### Checking User Access to Queues

To check a user's access to queues, follow these steps:

1. **Switch to the user account** (replace `<hawkid>` with the actual username):
    
```bash
su - <hawkid>
```
    
2. **Run `whichq`** to confirm the user's queue access:
    
```bash
whichq
```
    
    Example output:
    
```
You have access to the following queues:
----------------------------------------
HARRY
RON
UI
UI-DEVELOP
UI-GPU
UI-GPU-HM
UI-HM
UI-MPI
all.q
```
    

This will display all queues the user has access to.


- If `NCPU` is still incorrect, check SGE’s internal CPU reporting:
    
```bash
qconf -se <node-name> | grep num_proc
```
    
    - If `num_proc` is incorrect, proceed to Step 3.

---

### ✅ **Step 3: Reset Execution Host in Grid Engine**

If `qhost` is still incorrect, remove and re-add the execution host.

#### **On the Head Node**

```bash
qconf -de <node-name>
```

- This **removes** the execution host from SGE.

Re-add it:

```bash
qconf -ae
```

- This opens an editor. Ensure it contains:

```
hostname              <node-name>
load_scaling          NONE
complex_values        NONE
user_lists            NONE
xuser_lists           NONE
projects              NONE
xprojects             NONE
usage_scaling         NONE
report_variables      NONE
```

- Save and exit.

Restart `sge_execd` on the **compute node**:

```bash
sudo pkill -f sge_execd
/opt/sge/bin/lx-amd64/sge_execd
```

Verify if Grid Engine detects all CPUs:

```bash
qhost -h <node-name>
```

---

### ✅ **Step 4: Force Load Recalculation**

If the issue persists, force SGE to recalculate the CPU load values.

#### **On the Compute Node**

```bash
sudo rm -rf /opt/sge/default/spool/<node-name>/load*
```

Restart the execution daemon again:

```bash
sudo pkill -f sge_execd
/opt/sge/bin/lx-amd64/sge_execd
```

#### **On the Head Node**

Verify CPU allocation:

```bash
qhost -h <node-name>
```

---

# **Outcome**

- Grid Engine now detects the correct number of CPUs.
- `qhost` reports the expected `NCPU` value.
- Jobs are scheduled across all cores properly.
- No need for a system reboot.

### **Final Checklist**

|Step|Command|Where to Run|
|---|---|---|
|**Stop Execution Daemon**|`sudo pkill -f sge_execd`|**Compute Node**|
|**Restart Execution Daemon**|`/opt/sge/bin/lx-amd64/sge_execd`|**Compute Node**|
|**Verify CPU Reporting**|`qhost -h <node-name>`|**Head Node**|
|**Reset Execution Host**|`qconf -de <node-name> && qconf -ae`|**Head Node**|
|**Delete Load Reports & Restart**|`sudo rm -rf /opt/sge/default/spool/<node-name>/load*`|**Compute Node**|

### **Next Steps**

If the issue happens again, follow these troubleshooting steps to restore proper CPU detection in Grid Engine.

