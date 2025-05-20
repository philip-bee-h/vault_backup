---
title: Pi-hole Docker Compose Setup
created: 2024-10-28
modified: 2024-10-28
type: configuration
tags: [homelab, docker, network]
---

# Overview
This document outlines the Docker Compose setup for a self-hosted Pi-hole instance on a Raspberry Pi 4 running Pi OS Lite.

# Docker Compose Configuration
```yaml
version: "3"
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "80:80/tcp"
    environment:
      TZ: 'America/Chicago'
      WEBPASSWORD: 'Greengoose!'
    volumes:
      - '/home/pi/pihole/etc-pihole:/etc/pihole'
      - '/home/pi/pihole/etc-dnsmasq.d:/etc/dnsmasq.d'
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```

To properly organize and store this Docker Compose configuration for your Pi-hole setup in your homelab, consider placing it in the **003_resources > configurations > system-configs** directory, which appears to be intended for system-specific configurations.

### Suggested Documentation and Tagging
1. **Filename**: `pihole_docker_compose.md`
2. **Tagging**: Since this is a homelab note, include the `homelab` tag along with `docker` and `network` to categorize it as both a Docker and network resource.

### Suggested Documentation Format

Here’s a markdown structure for this note using the `resource-reference-template` or `command-reference-template`:


```yaml
version: "3"
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "80:80/tcp"
    environment:
      TZ: 'America/Chicago'
      WEBPASSWORD: 'Greengoose!'
    volumes:
      - '/home/pi/pihole/etc-pihole:/etc/pihole'
      - '/home/pi/pihole/etc-dnsmasq.d:/etc/dnsmasq.d'
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```

# Configuration Details
- **Environment Variables**:  
  - `TZ`: Sets the timezone.
  - `WEBPASSWORD`: Sets the web interface password.

- **Volumes**:
  - `/etc/pihole`: Stores Pi-hole configuration data.
  - `/etc/dnsmasq.d`: Stores DNS settings.

# Links
- [Pi-hole Docker Repository](https://github.com/pi-hole/docker-pi-hole)
- [Pi-hole Documentation](https://docs.pi-hole.net/)
