---
title: Proxmox Install Issue - `nomodeset` Solution
created: 2024-10-28
modified: 2024-10-28
type: troubleshooting
tags: [homelab, proxmox, troubleshooting, gui]
---

# Proxmox Install Issue: `nomodeset` Fix

## Issue
During the Proxmox installation, the graphical interface encountered issues.

## Solution
1. In the Proxmox boot menu, highlight the boot option and press `e` to edit.
2. Find the line starting with `linux` and add `nomodeset` at the end of this line.
3. Proceed with the boot to apply the change.

## Reference
- Video Tutorial: [Proxmox Install Issue Fix](https://www.youtube.com/watch?v=fXzyCM35TUc)
