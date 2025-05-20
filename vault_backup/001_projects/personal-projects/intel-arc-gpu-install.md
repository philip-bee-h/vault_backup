---
title: intel-arc-gpu-install
created: 2024-11-29
type: project
status: 
tags:
  - project
  - area-tag
  - uiowa
related_docs:
---
**Modified:**

To install the Intel ARC A310 GPU into your Dell PowerEdge T320 and get it working with Proxmox, follow these steps:

---

### 1. **Check Compatibility**

- Ensure your PowerEdge T320 has a PCIe x16 slot available for the ARC A310.
- Verify the power supply has sufficient wattage for the GPU and any additional power connectors required.
- Confirm the BIOS supports GPU passthrough if you intend to use this feature in Proxmox.

---

### 2. **Prepare the Proxmox Host**

- **Update Proxmox and Kernel:**

```
apt update && apt full-upgrade
```

Reboot to apply kernel updates:

```
reboot
```

- **Enable IOMMU:** Edit the kernel parameters:

```
nano /etc/default/grub
```

Add the following to `GRUB_CMDLINE_LINUX_DEFAULT`:

```
intel_iommu=on iommu=pt
```

Update GRUB:

```
update-grub
reboot
```

- **Verify IOMMU is Enabled:** After reboot, check:

```
dmesg | grep -e DMAR -e IOMMU
```

```sh
lspci | grep VGA
```
---

### 3. **Install the GPU**

- Power off the T320.
- Insert the ARC A310 into the PCIe x16 slot.
- Connect any necessary power cables from the PSU to the GPU.
- Boot the system and confirm the GPU is detected in the BIOS.

---

### 4. **Install Intel GPU Drivers**

- Intel ARC GPUs require modern drivers and firmware.
- Add Intel's GPU stack repository:

```
echo "deb [arch=amd64] https://repositories.intel.com/graphics/ubuntu focal main" | tee /etc/apt/sources.list.d/intel-graphics.list
wget -qO - https://repositories.intel.com/graphics/intel-graphics.key | apt-key add -
```

- Install the drivers:

```
apt update
apt install intel-opencl-icd intel-media-va-driver-non-free libmfx1 intel-media-va-driver
```

- Reboot the system:

```
reboot
```


---

### 5. **Confirm GPU is Detected**

- Use the following commands:

```
lspci | grep VGA
```

Look for an entry for the Intel ARC GPU.

```
dmesg | grep i915
```


---

### 6. **Set Up GPU Passthrough (Optional)**

If using the GPU in virtual machines:

- **Bind the GPU to VFIO:** Find the GPU's PCI ID:

```
lspci -nn
```

Edit `/etc/modprobe.d/vfio.conf`:

```
options vfio-pci ids=xxxx:yyyy
```

Replace `xxxx:yyyy` with the PCI ID.

- Update initramfs:
	
	```
	update-initramfs -u
	reboot
	```
	
- **Edit VM Configuration:** Add GPU passthrough details to your VM's `.conf` file in `/etc/pve/qemu-server/`.


---

### 7. **Test GPU Functionality**

- If the GPU will handle workloads, test with a benchmark or workload inside Proxmox or within a VM using passthrough.

Let me know if you encounter issues, and I can help troubleshoot!