---
title: Pi-hole Fallback DNS Configuration
created: 2024-10-28
modified: 2024-10-28
type: configuration
tags: [homelab, network, dns, pihole]
---

# Pi-hole Fallback DNS Configuration

If your Raspberry Pi running Pi-hole goes down, your network may lose DNS resolution. This document outlines fallback DNS options to prevent DNS outages.

## 1. Issues When Pi-hole Goes Down
- **Devices Lose Internet Access**  
  - **Symptoms**: Network devices may fail to load websites, use apps, or perform actions requiring DNS.
  - **Reason**: Without fallback DNS, devices won’t resolve domain names when Pi-hole is down.

## 2. Fallback DNS Options
To maintain DNS resolution, consider these fallback options:

### Option 1: Router-level DNS Fallback
- Configure Pi-hole as the primary DNS on your router and add a secondary DNS (e.g., 8.8.8.8 for Google, 1.1.1.1 for Cloudflare).
- **Steps**:
  1. Log into your router’s admin page.
  2. Set Pi-hole IP as the primary DNS.
  3. Add a secondary DNS.
  4. Save and apply.

### Option 2: Device-level DNS Fallback
- Set Pi-hole as the primary DNS and another public DNS server as secondary on individual devices.

### Option 3: DHCP-based Fallback
- Configure your DHCP server to distribute both Pi-hole and a secondary DNS to devices.

## 3. Preventive Measures
- **Automatic Restarts**: Set the Pi-hole Docker container to restart using:
  
```bash
docker run -d --name pihole --restart unless-stopped ...
```

- **Backup DNS Server**: Consider a second Raspberry Pi or low-power device as a backup DNS server.

## 4. DNS Cache on Devices

Some devices cache DNS, so they may still resolve DNS for a short time after Pi-hole goes down. This, however, is only a temporary solution.

By configuring fallback DNS, you ensure continuous network functionality even if Pi-hole or the Raspberry Pi becomes unavailable.