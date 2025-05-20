---
title: 03-designing-hpc-cluster
created: 2025-02-10
course:
  - Course name/code
resource: 
type: coursework
tags:
  - coursework
  - school
  - uiowa
  - active
  - conference-lci
---
## Cluster design guidelines

- keeping older nodes up slows down faster nodes
- plan it to be bottlenecked equally 

## Linux Cluster Architecture

- Headnode - brains of op
- Data transfer node?
	- globus, rsync,

![[designing-pc-cluster-01.png]]

- most log in nodes can run on a vm

## Compute nodes

- no swap space, he would rather the user's job dies swap declines performance by a ton
- rather have user know their job died and they should request more nodes


- does not recommend `/tmp` writing to disk slows stuff down prefers using ram

## Selecting processors

- More cores!= better performance
- Higher base frequency != better performance
- Hyperthreading shouldn't provide any benefit for HPC workloads
	- this really does not provide more benefit,
	- cluster jobs should be one at a time never context switching
	- over subscribing physical cores