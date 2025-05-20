

# Home Lab HPC Cluster Setup Guide

## Purpose

This setup is designed for **learning and experimentation**, not production HPC workloads. The goal is to gain hands-on experience with:

- Slurm (workload manager)
- Open OnDemand (web portal for job submission)
- Shared storage (e.g. NFS)
- Authentication (e.g. Munge)
- Environment modules
- MPI basics

---

## Hardware Requirements

### Controller Node (Head Node)
- Old server, laptop, or VM
- Minimum: 2–4 cores, 8+ GB RAM, 100GB+ storage
- Runs Slurm controller, NFS server, Munge, Open OnDemand

### Compute Nodes
- 1–2 physical or virtual machines
- Specs can be lower than the controller

### Networking
- Basic gigabit switch or LAN setup
- Nodes must be reachable by hostname or static IP

---

## Software Stack Overview

| Component         | Role                                   | Notes                             |
|------------------|----------------------------------------|-----------------------------------|
| OS               | Base Linux OS                          | Recommend: Rocky Linux 8/9        |
| Slurm            | Job scheduler                          | Install via package or source     |
| Munge            | Authentication for Slurm               | Shared key required               |
| NFS              | Shared home directory                  | Export from controller            |
| Open OnDemand    | Web interface                          | Optional, but useful              |
| Lmod/Modules     | Environment management                 | Recommended                       |
| MPI              | Parallel execution                     | OpenMPI or MPICH                  |

---

## Deployment Plan

### 1. Networking Setup
- Assign static IPs or reserved DHCP leases
- Define hostnames in `/etc/hosts` on all nodes

### 2. Install Base OS
- Use same OS and version on all nodes
- Minimal install with SSH access

### 3. NFS Configuration

On the controller node:
```bash
# /etc/exports
/home *(rw,sync,no_subtree_check,no_root_squash)
````

On compute nodes:

```bash
mkdir /home
mount headnode:/home /home
```

### 4. SSH Setup

```bash
ssh-keygen -t rsa
ssh-copy-id node01
```

### 5. Munge Setup

- Install `munge` on all nodes
    
- On controller:
    
    ```bash
    dd if=/dev/urandom bs=1 count=1024 > /etc/munge/munge.key
    chown munge:munge /etc/munge/munge.key
    chmod 400 /etc/munge/munge.key
    ```
    
- Copy the key to all nodes and start `munged`
    

### 6. Slurm Installation

- Generate config using: [https://slurm.schedmd.com/configurator.html](https://slurm.schedmd.com/configurator.html)
    
- Place `slurm.conf` in `/etc/slurm/`
    
- Start `slurmctld` (controller) and `slurmd` (nodes)
    

### 7. Job Submission Test

```bash
srun hostname
sbatch test.sh
squeue
```

### 8. Open OnDemand (Optional)

- Best on Rocky Linux 8/9
    
- Follow installation guide: [https://osc.github.io/ood-documentation/latest/](https://osc.github.io/ood-documentation/latest/)
    

### 9. Environment Modules and MPI

```bash
# Install environment modules
dnf install environment-modules
module avail

# Install OpenMPI
dnf install openmpi openmpi-devel
```

---

## Example Cluster Layout

```
+------------------+          +-----------------+
|  head (controller)| <-----> | node01 (compute)|
|  NFS, Slurmctld   |          | Slurmd          |
|  Munge, OOD       |          | Munge           |
+------------------+          +-----------------+
         |
         +-----------------+
         | node02 (compute)|
         | Slurmd + Munge  |
         +-----------------+
```

---

## Resources

- Slurm Documentation: [https://slurm.schedmd.com/documentation.html](https://slurm.schedmd.com/documentation.html)
    
- Slurm Config Generator: [https://slurm.schedmd.com/configurator.html](https://slurm.schedmd.com/configurator.html)
    
- Open OnDemand: [https://osc.github.io/ood-documentation/](https://osc.github.io/ood-documentation/)
    
- Lmod (Environment Modules): [https://lmod.readthedocs.io/en/latest/](https://lmod.readthedocs.io/en/latest/)
    
- MPI Tutorial: [https://mpitutorial.com/](https://mpitutorial.com/)
    

---

## Next Steps

-  Create Ansible inventory (optional)
    
-  Write sample `slurm.conf`
    
-  Add example MPI job
    
-  Explore monitoring tools (Grafana, Prometheus)
    

```

---

Let me know if you want to split this into multiple notes or build an Ansible playbook or configuration samples (like `slurm.conf`, `exports`, `ood_portal.yml`).
```