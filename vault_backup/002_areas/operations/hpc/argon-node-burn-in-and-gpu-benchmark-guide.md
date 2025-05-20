---
title: "argon-node-burn-in-and-gpu-benchmark-guide"
created: 2024-10-28
modified: 2024-10-28
type: process
system: hpc
tags:
  - argon
  - hpc
  - gpu
  - burn-in
  - benchmarking
---


### Suggested Location

Since this is a guide for validating node performance, it fits well under HPC operational processes:

```
002_areas/
└── operations/
    └── hpc/
        └── argon-node-burn-in-and-gpu-benchmark-guide.md
```

---

### Argon Node Burn-In and GPU Benchmarking Guide

#### Overview
This guide outlines the process for performing CPU burn-in testing and GPU benchmarking on Argon nodes. The tests are essential for hardware validation, ensuring stability before the node is added to the cluster.

---

## Section 1: Sequential CPU and GPU Burn-In

### 1.1 CPU Burn-In (For All Nodes)

1. **SSH into Node**:
   ```bash
   ssh [your_username]@node_address
   ```

2. **Start a New `tmux` Session**:
   ```bash
   tmux new -s burn_in_test
   ```

3. **Create Temporary Directory**:
   ```bash
   mkdir -p ~/burn_in_tests && cd ~/burn_in_tests
   ```

4. **Run CPU Burn-In Test**:
   ```bash
   sys_basher -t 256 -ho 48 > cpu_log.txt &
   ```
   For a heavier load:
   ```bash
   sys_basher -t 512 -ho 48 > cpu_log_heavy.txt &
   ```

5. **Detach `tmux` Session**:
   ```bash
   Ctrl-b d
   ```

6. **Reconnect to `tmux` (Optional)**:
   ```bash
   tmux attach -t burn_in_test
   ```

7. **Monitor CPU Test**:
   ```bash
   ps aux | grep [s]ys_basher
   ```

8. **Review CPU Log**:
   ```bash
   less cpu_log.txt
   ```

9. **Clean Up**:
   ```bash
   rm -rf ~/burn_in_tests
   ```

10. **Proceed to GPU Benchmarking (If GPU-equipped)**.

### 1.2 GPU Benchmarking (For GPU Nodes Only)

1. **Verify GPU Presence**:
   ```bash
   lspci | grep -i nvidia
   ```

2. **Load GPU Modules**:
   ```bash
   module load stack gpu-burn
   ```

3. **Start a New `tmux` Session (Optional)**:
   ```bash
   tmux new -s gpu_burn_in
   ```

4. **Create Temporary Directory**:
   ```bash
   mkdir -p ~/gpu_burn_tests && cd ~/gpu_burn_tests
   ```

5. **Run GPU Burn-In Test**:
   ```bash
   gpu_burn 3600 > gpu_burn.txt &
   ```

6. **Detach `tmux` Session**:
   ```bash
   Ctrl-b d
   ```

7. **Monitor GPU Test**:
   ```bash
   ps aux | grep [g]pu_burn
   ```

8. **Review GPU Log**:
   ```bash
   less gpu_burn.txt
   ```

9. **Clean Up**:
   ```bash
   rm -rf ~/gpu_burn_tests
   ```

---

## Section 2: Concurrent CPU and GPU Burn-In

If the node is GPU-equipped and you want to perform both tests simultaneously, follow these steps:

1. **SSH into Node**:
   ```bash
   ssh [your_username]@node_address
   ```

2. **Start a Single `tmux` Session**:
   ```bash
   tmux new -s burn_in_test
   ```

3. **Create Temporary Directory**:
   ```bash
   mkdir -p ~/burn_in_tests && cd ~/burn_in_tests
   ```

4. **Run CPU Burn-In Test**:
   ```bash
   sys_basher -t 256 -ho 48 > cpu_log.txt &
   ```

5. **Run GPU Burn-In Test**:
   ```bash
   module load stack gpu-burn
   gpu_burn 3600 > gpu_burn.txt &
   ```

6. **Detach `tmux` Session**:
   ```bash
   Ctrl-b d
   ```

7. **Monitor Both Tests**:
   ```bash
   ps aux | grep -E '[s]ys_basher|[g]pu_burn'
   ```

8. **Review Logs**:
   - **CPU Log**: `less cpu_log.txt`
   - **GPU Log**: `less gpu_burn.txt`

9. **Clean Up**:
   ```bash
   rm -rf ~/burn_in_tests
   ```

---

## Conclusion

Following this guide ensures that each node’s CPU and GPU hardware has been thoroughly tested for stability and performance. Running both tests sequentially or concurrently provides flexibility depending on the node’s configuration and your testing preferences.