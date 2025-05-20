---
title: Flashing an ISO to a USB on macOS
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: quick-reference
tags:
  - macos
  - iso-flashing
  - usb-setup
  - active
related_docs:
 
---

# Quick Overview
A step-by-step guide to flashing an ISO image to a USB drive on macOS using Terminal.

## Prerequisites
- **ISO Image**: e.g., `example.iso`
- **USB Drive**: Minimum 8GB, all data will be erased
- **Terminal Access** on macOS

## Steps

1. **Insert USB Drive**: Connect the USB to an available port.
2. **Open Terminal**: Use `Applications > Utilities > Terminal` or `Cmd + Space` and search "Terminal".
3. **Identify USB Drive**: Run `diskutil list` and locate `/dev/diskN` (e.g., `/dev/disk2`).
4. **Unmount USB**: Execute `diskutil unmountDisk /dev/diskN`.
5. **Convert ISO to IMG (If Necessary)**:

```zsh
hdiutil convert -format UDRW -o example.img path/to/example.iso
```

6. **Write to USB Using `dd`**:

```zsh
sudo dd if=path/to/example.img of=/dev/rdiskN bs=1m
```
   
1. **Eject USB**: Run `diskutil eject /dev/diskN`.

## Verification (Optional)
- Restart Mac, hold `Option (⌥)`, select the USB as the startup disk.

# Troubleshooting Tips
- **Permission Errors**: Use `sudo`.
- **Device not recognized**: Try reinserting or using a different port.

---

### **Additional Resources**
