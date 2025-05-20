---
title: "argon-nodes-troubleshooting-and-management"
created: 2024-10-28
modified: 2024-10-28
type: reference
system: hpc
tags:
  - hpc
  - troubleshooting
  - sge
  - argon
  - diagnostics
---
## **Overview of SGE (Sun Grid Engine)**

Sun Grid Engine (SGE) manages job scheduling in high-performance computing (HPC) environments, handling job execution, prioritization, and resource allocation.

---

## **Key Definitions**

- **Queue Manager (SGE)**: Controls job operations and allocates resources.
- **Queue**: Collection of queue instances on specific compute nodes.
- **Host Group**: Group of compute nodes assigned to specific queues.
- **ACL (Access Control List)**: Users with permission to submit jobs to specific queues.
- **Job**: Entity containing both configuration and executable shell commands.

---

## **Common Commands for SGE Management**

### **1. qstat - Job Status Viewer**

Commands for listing job statuses and detailed job information.

- **Basic Usage**:
```bash
qstat
```

- **Detailed Job Information**:
    - `qstat -j job_id` - Comprehensive job details.
    - `qstat -explain [a|c|A|E]` - Explain states, with reasons for pending/alarm states.
    - `qstat -u` - Lists jobs for the current user.
    - `qstat -l h` - Lists jobs by host.
    - `qstat -l h="$NODE"` - Lists jobs running on a specific node.
    - `qstat -f | grep argon-itf-bx43-03` - Filters jobs to show only those running on the specified node.
    - `qstat -explain a -f | grep -C 8 argon-itf-bx43-03` - Shows 8 lines before and after the specified node in the full job list, including reasons for job delays.
    - `qstat -explain a -f | grep -B 8 -A 8 argon-itf-bx43-03` - Same as above but explicitly adds 8 lines before and after the search result.
    - `qstat -explain a -f | awk '/argon-itf-bx43-03/{for(i=-8; i<=8; i++) print (NR+i) " " $0}'` - Extracts and prints job details with surrounding context for the specified node.

---

### **2. qmod - Queue Modification Tool**

Use `qmod` for enabling, disabling, and managing jobs in queues (requires root privileges).

```sh
sudo su -
```

- **Enable Queue**:

```bash
qmod -e queue_name@hostname
```

- **Disable Queue**:

```bash
qmod -d queue_name@hostname
```

- **Suspend Job**:

```bash
qmod -sj job_id
```

- **Disable all queues on a node before powering down**:

```bash
qmod -d *@hostname
```


---

### **3. qhost - Host Status Viewer**

Monitor host and queue statuses.

- **General Host Info**:

```bash
qhost
```

- **Host and Queue Details**:

```bash
qhost -q -h argon-itf-bx48-22
```

- **Check resource usage of a specific node, excluding zero values**:

```bash
qhost -F -h argon-itf-ca43-01 | grep -v '=0.000000'
```

_(Displays actual resource usage, including GPUs, CPUs, and memory.)_

---

### **4. qsub - Job Submission Tool**

Submit jobs to SGE.

- **Submit a job from a script**:

```bash
qsub script.sh
```

- **Submit to a specific queue**:

```bash
qsub -q queue_name script.sh
```


---

### **5. qdel - Delete Jobs**

Remove jobs from the queue.

- **Delete a job**:

```bash
qdel job_id
```
  

---

### **6. qacct - Accounting Information**

Retrieve completed job accounting information.

- **View job accounting details**:

```bash
qacct -j job_id
```


---

### **7. qconf - SGE Configuration Tool**

Configure SGE queues, hosts, and other resources.

- **View Queue Configuration**:

```bash
qconf -sq queue_name
```

- **Add Execution Host**:

```bash
qconf -ae host
```


---

## **Common Issues and Troubleshooting Tips**

- **Job Stuck in Queue**: Use `qstat -f` to check system-wide status.
- **Resource Overload**: Check host load using `qhost` and adjust job resource requests as needed.

---

## **Troubleshooting Scenarios**

1. **Job Preemption**: Often in 'all.q' where high-priority jobs preempt others.
2. **Queue Configuration Issues**: Misconfigured queues or incorrect ACLs can prevent job submission.

---

## **Practical Examples and Code Snippets**

### **Finding Node Errors**

List queues or nodes in error states:

```bash
qstat -f | awk '$6~/[cdsuE]/'
```

### **Investigate SGE Status**

Check for issues with specific hosts:

```bash
qhost -h argon-itf-bx48-22 -q
qhost -h argon-itf-bx48-22 -j | grep -v SLAVE
```

### **Explain SGE Alarm States**

Diagnose nodes in alarm states with `qstat -explain`:

```bash
qstat -explain a -q FERBIN@argon-lc-f14-15
```

### **Check Node Status**

Run SSH, Ping, and IPMI commands for detailed diagnostics.

#### **Example Commands for Node Status Checks:**

1. **Check Power and Fault Status**:

```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H argon-lc-h21-34.ipmi chassis status
```

2. **View Recent SEL Entries**:

```bash
sudo ipmitool -f /root/ipmipass -U ARGON -H argon-lc-h21-34.ipmi sel elist
```

### **View System Logs for Compute Nodes**

Access system logs stored on a central server:

```bash
sudo ls /srv/logs/argon-itf-bx53-11
```

---

### **Additional Host Query Example**

Check the status of a specific host and its queues:

```bash
qhost -h argon-itf-ca44-03 -q
```

#### **Example Output:**

```
HOSTNAME                ARCH         NCPU NSOC NCOR NTHR  LOAD  MEMTOT  MEMUSE  SWAPTO  SWAPUS
------------------------------------------------------------------------------------------------
global                  -               -    -    -    -     -       -       -       -       -
argon-itf-ca44-03       lx-amd64      128    2   64  128  0.01  503.6G    6.6G     0.0     0.0
   WEIRANWANG-GROUP     BIPC  0/48/128   
   all.q                BIPC  0/0/128       S
```

_(Shows CPU count, memory usage, and queue states for the node.)_

---

This guide serves as a structured reference for managing and troubleshooting Argon HPC nodes, focusing on **SGE job management, queue monitoring, host diagnostics, and system troubleshooting**. Let me know if you’d like to add more details or refine any section!