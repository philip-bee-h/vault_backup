---
title: 00-lci-info
created: 2025-02-10
type: general
tags:
  - uiowa
  - conference-lci
---
### slack channels
```sh
https://join.slack.com/t/sighpc-syspros/shared_invite/zt-2zh0x624m-bHiXR0icRqJ3XcSHwtK0XA
```

### Schedulers in use
- slurm
- PBS Pro

### Provisioning 
- xcat
- warewulf
- confluent
- foreman
- initramfs-tools
- dracut


### Storage
- VAST
- gpfs?
- versity
- ceph does not talk rdma (what infinaband uses) - kansas used pcp 100g network

### Monitoring
- they have graphs for amouts written and read daily
- lustre
- do we need auto logging??

### Notes/Question
- Spack Huston Rogers
	- find in spack slack
- do we have SLA's - revisit?
- virtilizing - proxmox
	- lxc for 
		- slurm
		- globus
		- check with JD
- Schedulers
	- over view of how we tune this and decide priority. 
- `mpich.org` - for mpi resource
- User SUpport
	- MSU hosting own ticketing RT
	- others using Jira, team dynamics
	- Using email connector may be good idea
- MSU using team dynamics for documentation


---
## 05-hpc-storage-part-1-2
- are users copying in to scratch to run jobs then copying ity back?
	- there is a trend of scratch and project merging 
- tuned performance for read on home and software
- setting inodes quota?
- they have 15 min monitoring for quota to tracking quota to know
- job stats for ceph?! able to tie job numbers or location to io
- disable pre-fetch for ai jobs so it does not predictive fetching to cache
- jenkins runs micro benchmarks every hour
	- very small, io cycles - 10 sec
- run micro benchmarks before turning system back to users after maint Benchmarking slide

---

## 07-cluster-stack-basics
- telemetry over management network
- NCSA
	- storage 
		- ansible
	- hpc 
		- puppet
		- xcat - provisioning 
- Managment node
	- what is our managment node 
	- dedicated logging server
		- using greylog
	- management might be log
	- using clush across the parallel shell
- login in node
	- resource manager - Munge?
	- do we have log forwarding to look at why thinks failed in stateless 
		- kdump
	- containorization - for software
		- apptainer
		- no privilege fullstop

## Configuration Managment
- 2 main ways we provision nodes
	- stateful
	- stateless

## schedulers
- What is a scheduler 
- Why do we need to use schedulers
### Slurm

```sh
sacct -B -j 59
```

```shell
sacct -j 59 -o JobID,User,JobName,Workdir -P
```

```sh
#!/bin/bash
#
#SBATCH -J hello
#SBATCH -N 1
#SBATCH -n 1
#SBATCH -t 00:01:00
#SBATCH --mem=1M
#SBATCH -o %x-%j.out
#SBATCH -e %x-%j.out

echo "Hello, world"
echo "Going to sleep. Zzzzzz..."
sleep 60
echo "I'm awake. Goodbye"
```

- use sleep statment when testing code to give you time to check scheduler

```sh
mpitun -np 2 mpi-hello
```

## Monitoring 
- what api is avail - when talking with vendors
- monitor ib connection

## HPC Networking

## Software management
- contrib
	- apps from users - weather module example
- 

https://hpc-syspros-basics.github.io/Contributing/Contribution_guide/


https://help.obsidian.md/Getting+started/Update+Obsidian#Installer+updates
