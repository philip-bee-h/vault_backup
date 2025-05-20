---
title: homelab-thoughts
created: 2024-11-02
type: general
tags:
  - homelab
  - network
  - storage
  - monitoring
  - active
---


# Homelab Plan

- pve-prod
	- proxmox production env - dell power edge T320
		- media server
		- and other services vms and lxc
		- vs code server
- pve-dev
	- proxmox dev env - dell prescion tower
		- place to explor and test apps before putting them in to prod
- pi-srvr
	- raspberry pi 4 server 
	- used for monitoring service
	- homepage, grafana loki, promethous 
- ws-mediahub
	- dell optiplex media ripping station
	- will use for ripping dvds
	- will likely house my arrr stack apps
- plex-srvr
	- dell vosro with entrprize gpu
	- plex server
- ws-daily
	- daily workstation
	- lenovo t14 gen2
	- used to interact with everything





# Homelab Notes

## Services (Planned/Current)

- **Homepage**: Centralized dashboard for all homelab services
- **Network Filtering**: Pi-hole for ad-blocking and DNS filtering
- **Storage**: ZFS-based server for secure, redundant storage
- **Blog Server**: Jekyll blog on GitHub or Docker-based
- **Media Server**: Plex for streaming
- **Monitoring & Analytics**:
  - Grafana for data visualization
  - Prometheus for metrics collection
- **Documentation**: Wiki or MkDocs for internal documentation
- **Diagramming**: Draw.io for network and system diagrams
- coding - va code server to wo


### Additions
-  IT tools https://noted.lol/it-tools/


## Hardware Inventory

### Dell Precision T3610 - lab
- **RAM**: 32GB - 64G
- **Storage**: 4 x 500GB SSDs (ZFS setup)
  - **1 SSD** for OS
  - **3 SSDs** for ZFS pool
- **OS**: Proxmox (Virtualization and resource allocation)
	- ubuntu server for blog on githubpages with jekyll

### Dell PowerEdge T320
- **RAM**: ~40GB
- **Storage**:
  - 8 x 1TB HDDs
  - 2 x 1TB SSDs in dedicated bays
- **OS**: proxmox
	- planning to host my media storage on here with an ubunut server/samba and zfs

### Dell OptiPlex 9010
- **RAM**: 32GB
- **Storage**: 1 x 500GB SSD
- **Use Case**:  Linux installation for MakeMKV, DVD ripping station

### Dell Vostro
- **RAM**: 16GB
- **Storage**: 1TB NVMe
- **GPU**: Enterprise NVIDIA GPU
- **Use Case**:  Plex media server with hardware-accelerated streaming

### Lenovo T450s
- **RAM**: 16GB
- **Storage**: 500GB SSD
- **Use Case**: Dev-test env server or potential Docker node
	- a place I can test docker containers not in prod

### Lenovo T14 (Workstation)
- **RAM**: 16GB
- **Storage**: 1TB SSD
- **OS**: Fedora (Daily driver)

### Raspberry Pi 4
- **RAM**: 4GB
- **Storage**: 2 x 500GB SSDs
- **OS**: Raspberry Pi OS Lite
- **Use Case**: Docker server for lightweight services or experimentation
	- hompage for homelab dashboard


## Deployment Plans

1. **Network Setup**:
   - Pi-hole on the Raspberry Pi for ad-blocking and DNS control
   - Centralized homepage on a VM for easy access to services

2. **Storage Setup**:
   - ZFS pool configuration on the Dell Precision T3610 for general file storage
   - Consider setting up a second storage node on the PowerEdge T320 with ZFS or Ceph for redundancy

3. **Media Streaming**:
   - Use the Dell Vostro with NVIDIA GPU as a dedicated Plex server for video streaming
   - Set up the OptiPlex 9010 for DVD ripping and media organization (possibly integrated with Plex)

4. **Monitoring and Documentation**:
   - Deploy Grafana and Prometheus on a Docker container for monitoring across nodes
   - Run MkDocs or Wiki.js for documenting network setup, usage, and other homelab instructions on the Raspberry Pi

5. **Blog and Documentation**:
   - Docker container for Jekyll blog (if not using GitHub Pages directly)
   - Draw.io for homelab diagramming on a central VM

---

## Self-Hosted Services to Consider

### Networking & Security
- **OPNsense**: Open-source firewall and router platform with features like intrusion detection, traffic shaping, and VPN.
- **WireGuard**: Lightweight, high-speed VPN for secure remote access and site-to-site connectivity.
- **Netbird**: Simplified, peer-to-peer WireGuard-based VPN for secure connections without complex configuration.
- **PiVPN**: Easy setup tool for creating a WireGuard or OpenVPN server on a Raspberry Pi.

### Development & Code Management
- **Gitea**: Lightweight, self-hosted Git service, perfect for personal project repositories and version control.
- **Jenkins**: CI/CD automation server to build, test, and deploy code within your homelab.
- **GitLab**: Feature-rich alternative to GitHub for hosting and managing projects, with built-in CI/CD pipelines.

### Backup & File Management
- **Nextcloud**: File storage and collaboration platform for private cloud storage, document sharing, and synchronization.
- **Seafile**: File hosting service with built-in file syncing, encryption, and an easy-to-use interface.
- **Duplicati**: Backup solution that can securely back up files to cloud storage, file servers, or external drives.

### Monitoring & Logging
- **Zabbix**: Enterprise-level monitoring solution with SNMP support, alerting, and customizable dashboards.
- **ELK Stack** (Elasticsearch, Logstash, Kibana): Centralized logging and analysis solution for real-time log data.
- **Netdata**: Resource monitoring with real-time metrics, ideal for quick snapshots of your system’s health.

### Networking Tools
- **Portainer**: GUI for managing Docker containers, images, and networks, simplifying container management.
- **Caddy**: Web server and reverse proxy with automatic HTTPS, suitable for hosting various web services securely.
- **Nginx Proxy Manager**: Simple proxy server with GUI, ideal for handling SSL and routing requests to other services.

### Collaboration & Documentation
- **Wiki.js**: Self-hosted wiki platform for team documentation with an easy-to-use editor and markdown support.
- **BookStack**: Documentation platform with a book-style organization, ideal for structured and hierarchical documentation.
- **Jellyfin**: Self-hosted media server alternative to Plex, supporting movies, music, and more.

### Home Automation
- **Home Assistant**: Open-source home automation software for integrating smart devices, scheduling, and automation workflows.
- **OpenHAB**: Alternative home automation platform compatible with a wide range of IoT devices and protocols.

### Privacy & Security Tools
- **Bitwarden**: Self-hosted password manager for secure password storage and sharing within your network.
- **Vaultwarden** (Bitwarden fork): Lightweight, self-hosted version of Bitwarden, optimized for low-resource devices.
- **Guacamole**: Web-based remote desktop gateway, allowing secure remote access to devices over RDP, SSH, and VNC.
