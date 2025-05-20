---
title: pi-mon-yavin
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: resource
category:
  - homelab
  - hardware
tags:
  - homelab
  - raspberry-pi
  - configuration
  - active
related_docs:
---


# pi-mon-yavin Configuration

## System Overview
- **OS**: Raspberry Pi OS Lite (configured as a server)
- **Hostname**: `pi-mon-yavin`
- **Primary User**: `pbh`
- **SSH Access**: `ssh pbh@192.168.10.6`

### SSH Connection Example
To access the server via SSH, use the following command:

```shell
ssh pbh@192.168.10.6
```


## Hosted Services
---

| Service  | Port | Config                     |
| -------- | ---- | -------------------------- |
| Homepage | 3000 | [[homepage-docker-config]] |
